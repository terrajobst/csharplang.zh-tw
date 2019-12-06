---
ms.openlocfilehash: 4d6d28a3127bc701867afe157aa5496377a06f69
ms.sourcegitcommit: 63d276488c9770a565fd787020783ffc1d2af9d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868000"
---
# <a name="conversions"></a><span data-ttu-id="9cf32-101">轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-101">Conversions</span></span>

<span data-ttu-id="9cf32-102">***轉換***可讓運算式被視為屬於特定類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-102">A ***conversion*** enables an expression to be treated as being of a particular type.</span></span> <span data-ttu-id="9cf32-103">轉換可能會使給定類型的運算式被視為具有不同的類型，或者，它可能會導致沒有類型的運算式取得類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-103">A conversion may cause an expression of a given type to be treated as having a different type, or it may cause an expression without a type to get a type.</span></span> <span data-ttu-id="9cf32-104">轉換可以是***隱含***或***明確***的，這會決定是否需要明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-104">Conversions can be ***implicit*** or ***explicit***, and this determines whether an explicit cast is required.</span></span> <span data-ttu-id="9cf32-105">例如，從類型 `int` 到類型 `long` 的轉換是隱含的，因此類型 `int` 的運算式可以隱含地視為類型 `long`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-105">For instance, the conversion from type `int` to type `long` is implicit, so expressions of type `int` can implicitly be treated as type `long`.</span></span> <span data-ttu-id="9cf32-106">相反的轉換（從類型 `long` 到類型 `int`）是明確的，因此需要明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-106">The opposite conversion, from type `long` to type `int`, is explicit and so an explicit cast is required.</span></span>

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

<span data-ttu-id="9cf32-107">某些轉換是由語言所定義。</span><span class="sxs-lookup"><span data-stu-id="9cf32-107">Some conversions are defined by the language.</span></span> <span data-ttu-id="9cf32-108">程式也可以定義自己的轉換（[使用者定義的轉換](conversions.md#user-defined-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-108">Programs may also define their own conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

## <a name="implicit-conversions"></a><span data-ttu-id="9cf32-109">隱含轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-109">Implicit conversions</span></span>

<span data-ttu-id="9cf32-110">下列轉換會分類為隱含轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-110">The following conversions are classified as implicit conversions:</span></span>

*  <span data-ttu-id="9cf32-111">身分識別轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-111">Identity conversions</span></span>
*  <span data-ttu-id="9cf32-112">隱含數值轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-112">Implicit numeric conversions</span></span>
*  <span data-ttu-id="9cf32-113">隱含列舉轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-113">Implicit enumeration conversions</span></span>
*  <span data-ttu-id="9cf32-114">隱含插入字串轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-114">Implicit interpolated string conversions</span></span>
*  <span data-ttu-id="9cf32-115">隱含可為 null 的轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-115">Implicit nullable conversions</span></span>
*  <span data-ttu-id="9cf32-116">Null 常值轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-116">Null literal conversions</span></span>
*  <span data-ttu-id="9cf32-117">隱含參考轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-117">Implicit reference conversions</span></span>
*  <span data-ttu-id="9cf32-118">裝箱轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-118">Boxing conversions</span></span>
*  <span data-ttu-id="9cf32-119">隱含動態轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-119">Implicit dynamic conversions</span></span>
*  <span data-ttu-id="9cf32-120">隱含常數運算式轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-120">Implicit constant expression conversions</span></span>
*  <span data-ttu-id="9cf32-121">使用者定義的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-121">User-defined implicit conversions</span></span>
*  <span data-ttu-id="9cf32-122">匿名函數轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-122">Anonymous function conversions</span></span>
*  <span data-ttu-id="9cf32-123">方法群組轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-123">Method group conversions</span></span>

<span data-ttu-id="9cf32-124">隱含轉換可能會在各種情況下發生，包括函數成員調用（[動態多載解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）、轉換運算式（[cast 運算式](expressions.md#cast-expressions)）和指派（[指派運算子](expressions.md#assignment-operators)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-124">Implicit conversions can occur in a variety of situations, including function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), and assignments ([Assignment operators](expressions.md#assignment-operators)).</span></span>

<span data-ttu-id="9cf32-125">預先定義的隱含轉換一律會成功，而且永遠不會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9cf32-125">The pre-defined implicit conversions always succeed and never cause exceptions to be thrown.</span></span> <span data-ttu-id="9cf32-126">適當設計的使用者定義隱含轉換也應該展現這些特性。</span><span class="sxs-lookup"><span data-stu-id="9cf32-126">Properly designed user-defined implicit conversions should exhibit these characteristics as well.</span></span>

<span data-ttu-id="9cf32-127">基於轉換的目的，`object` 和 `dynamic` 的類型會被視為相等。</span><span class="sxs-lookup"><span data-stu-id="9cf32-127">For the purposes of conversion, the types `object` and `dynamic` are considered equivalent.</span></span>

<span data-ttu-id="9cf32-128">不過，動態轉換（[隱含動態轉換](conversions.md#implicit-dynamic-conversions)和[明確動態轉換](conversions.md#explicit-dynamic-conversions)）只適用于 `dynamic` 類型的運算式（[動態類型](types.md#the-dynamic-type)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-128">However, dynamic conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)) apply only to expressions of type `dynamic` ([The dynamic type](types.md#the-dynamic-type)).</span></span>

### <a name="identity-conversion"></a><span data-ttu-id="9cf32-129">身分識別轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-129">Identity conversion</span></span>

<span data-ttu-id="9cf32-130">身分識別轉換會從任何類型轉換成相同的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-130">An identity conversion converts from any type to the same type.</span></span> <span data-ttu-id="9cf32-131">這種轉換存在的情況下，您可以將已經具有必要類型的實體視為可轉換成該類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-131">This conversion exists such that an entity that already has a required type can be said to be convertible to that type.</span></span>

*  <span data-ttu-id="9cf32-132">由於 `object` 和 `dynamic` 被視為相等，因此在 `object` 和 `dynamic`之間，以及將所有出現的 `dynamic` 取代為 `object`時，兩者之間會有識別轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-132">Because `object` and `dynamic` are considered equivalent there is an identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing all occurrences of `dynamic` with `object`.</span></span>

### <a name="implicit-numeric-conversions"></a><span data-ttu-id="9cf32-133">隱含數值轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-133">Implicit numeric conversions</span></span>

<span data-ttu-id="9cf32-134">隱含數值轉換包括：</span><span class="sxs-lookup"><span data-stu-id="9cf32-134">The implicit numeric conversions are:</span></span>

*  <span data-ttu-id="9cf32-135">從 `sbyte` 到 `short`、`int`、`long`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-135">From `sbyte` to `short`, `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-136">從 `byte` 到 `short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-136">From `byte` to `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-137">從 `short` 到 `int`、`long`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-137">From `short` to `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-138">從 `ushort` 到 `int`、`uint`、`long`、`ulong`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-138">From `ushort` to `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-139">從 `int` 到 `long`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-139">From `int` to `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-140">從 `uint` 到 `long`、`ulong`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-140">From `uint` to `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-141">從 `long` 到 `float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-141">From `long` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-142">從 `ulong` 到 `float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-142">From `ulong` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-143">從 `char` 到 `ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-143">From `char` to `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-144">從 `float` 到 `double`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-144">From `float` to `double`.</span></span>

<span data-ttu-id="9cf32-145">從 `int`、`uint`、`long`或 `ulong` 轉換成 `float` 和從 `long` 或 `ulong` 到 `double` 可能會導致精確度遺失，但絕不會造成損失。</span><span class="sxs-lookup"><span data-stu-id="9cf32-145">Conversions from `int`, `uint`, `long`, or `ulong` to `float` and from `long` or `ulong` to `double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="9cf32-146">其他隱含數值轉換永遠不會遺失任何資訊。</span><span class="sxs-lookup"><span data-stu-id="9cf32-146">The other implicit numeric conversions never lose any information.</span></span>

<span data-ttu-id="9cf32-147">`char` 類型沒有隱含轉換，因此其他整數類型的值不會自動轉換成 `char` 類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-147">There are no implicit conversions to the `char` type, so values of the other integral types do not automatically convert to the `char` type.</span></span>

### <a name="implicit-enumeration-conversions"></a><span data-ttu-id="9cf32-148">隱含列舉轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-148">Implicit enumeration conversions</span></span>

<span data-ttu-id="9cf32-149">隱含列舉轉換可讓*decimal_integer_literal* `0` 轉換成任何*enum_type* ，以及其基礎類型為*enum_type*的任何*nullable_type* 。</span><span class="sxs-lookup"><span data-stu-id="9cf32-149">An implicit enumeration conversion permits the *decimal_integer_literal* `0` to be converted to any *enum_type* and to any *nullable_type* whose underlying type is an *enum_type*.</span></span> <span data-ttu-id="9cf32-150">在後者的情況下，轉換會藉由轉換成基礎*enum_type*並將結果包裝起來（[可為 null 的類型](types.md#nullable-types)）來進行評估。</span><span class="sxs-lookup"><span data-stu-id="9cf32-150">In the latter case the conversion is evaluated by converting to the underlying *enum_type* and wrapping the result ([Nullable types](types.md#nullable-types)).</span></span>

### <a name="implicit-interpolated-string-conversions"></a><span data-ttu-id="9cf32-151">隱含插入字串轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-151">Implicit interpolated string conversions</span></span>

<span data-ttu-id="9cf32-152">隱含插入字串轉換可讓*interpolated_string_expression* （[字串插值](expressions.md#interpolated-strings)）轉換成 `System.IFormattable` 或 `System.FormattableString` （這會實 `System.IFormattable`）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-152">An implicit interpolated string conversion permits an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)) to be converted to `System.IFormattable` or `System.FormattableString` (which implements `System.IFormattable`).</span></span>

<span data-ttu-id="9cf32-153">套用此轉換時，字串值不是由插入字串所組成。</span><span class="sxs-lookup"><span data-stu-id="9cf32-153">When this conversion is applied a string value is not composed from the interpolated string.</span></span> <span data-ttu-id="9cf32-154">相反地，會建立 `System.FormattableString` 的實例，如[字串插值](expressions.md#interpolated-strings)中所述。</span><span class="sxs-lookup"><span data-stu-id="9cf32-154">Instead an instance of `System.FormattableString` is created, as further described in [Interpolated strings](expressions.md#interpolated-strings).</span></span>

### <a name="implicit-nullable-conversions"></a><span data-ttu-id="9cf32-155">隱含可為 null 的轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-155">Implicit nullable conversions</span></span>

<span data-ttu-id="9cf32-156">在不可為 null 的實數值型別上操作的預先定義隱含轉換，也可以搭配這些類型的可為 null 形式使用。</span><span class="sxs-lookup"><span data-stu-id="9cf32-156">Predefined implicit conversions that operate on non-nullable value types can also be used with nullable forms of those types.</span></span> <span data-ttu-id="9cf32-157">針對從不可為 null 的實值型別轉換成不可為 null 之實值型別的每個預先定義的隱含識別和數值轉換 `S` 為不能為 null 的實數值型別 `T`，會存在下列隱含可為 null</span><span class="sxs-lookup"><span data-stu-id="9cf32-157">For each of the predefined implicit identity and numeric conversions that convert from a non-nullable value type `S` to a non-nullable value type `T`, the following implicit nullable conversions exist:</span></span>

*  <span data-ttu-id="9cf32-158">從 `S?` 到 `T?`的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-158">An implicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="9cf32-159">從 `S` 到 `T?`的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-159">An implicit conversion from `S` to `T?`.</span></span>

<span data-ttu-id="9cf32-160">根據從 `S` 到 `T` 的基礎轉換，評估隱含可為 null 的轉換，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9cf32-160">Evaluation of an implicit nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="9cf32-161">如果可為 null 的轉換是從 `S?` 到 `T?`：</span><span class="sxs-lookup"><span data-stu-id="9cf32-161">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="9cf32-162">如果來源值為 null （`HasValue` 屬性為 false），則結果會是 `T?`類型的 null 值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-162">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="9cf32-163">否則，會將轉換評估為從 `S?` 解除包裝為 `S`，後面接著從 `S` 到 `T`的基礎轉換，後面接著從 `T` 到 `T?`的包裝（[可為 null 的類型](types.md#nullable-types)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-163">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping ([Nullable types](types.md#nullable-types)) from `T` to `T?`.</span></span>

*  <span data-ttu-id="9cf32-164">如果可為 null 的轉換是從 `S` 到 `T?`，則會將轉換評估為從 `S` 到 `T` 的基礎轉換，然後再從 `T` 換成 `T?`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-164">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>

### <a name="null-literal-conversions"></a><span data-ttu-id="9cf32-165">Null 常值轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-165">Null literal conversions</span></span>

<span data-ttu-id="9cf32-166">從 `null` 常值到任何可為 null 的型別都有隱含的轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-166">An implicit conversion exists from the `null` literal to any nullable type.</span></span> <span data-ttu-id="9cf32-167">這項轉換會產生給定可為 null 類型的 null 值（[可為](types.md#nullable-types)null 的類型）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-167">This conversion produces the null value ([Nullable types](types.md#nullable-types)) of the given nullable type.</span></span>

### <a name="implicit-reference-conversions"></a><span data-ttu-id="9cf32-168">隱含參考轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-168">Implicit reference conversions</span></span>

<span data-ttu-id="9cf32-169">隱含參考轉換包括：</span><span class="sxs-lookup"><span data-stu-id="9cf32-169">The implicit reference conversions are:</span></span>

*  <span data-ttu-id="9cf32-170">從任何*reference_type*到 `object` 並 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-170">From any *reference_type* to `object` and `dynamic`.</span></span>
*  <span data-ttu-id="9cf32-171">從任何*class_type* `S` 到任何*class_type*的 `T`，提供 `S` 衍生自 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-171">From any *class_type* `S` to any *class_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="9cf32-172">從任何*class_type* `S` 到任何*interface_type* `T`，提供 `S` 執行 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-172">From any *class_type* `S` to any *interface_type* `T`, provided `S` implements `T`.</span></span>
*  <span data-ttu-id="9cf32-173">從任何*interface_type* `S` 到任何*interface_type*的 `T`，提供 `S` 衍生自 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-173">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="9cf32-174">從具有元素類型的*array_type* `S` `SE` 至專案類型 `T` 的*array_type* `TE`，前提是下列所有條件皆成立：</span><span class="sxs-lookup"><span data-stu-id="9cf32-174">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="9cf32-175">`S` 和 `T` 只有元素類型不同。</span><span class="sxs-lookup"><span data-stu-id="9cf32-175">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="9cf32-176">換句話說，`S` 和 `T` 的維度數目相同。</span><span class="sxs-lookup"><span data-stu-id="9cf32-176">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="9cf32-177">`SE` 和 `TE` 都是*reference_type*s。</span><span class="sxs-lookup"><span data-stu-id="9cf32-177">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="9cf32-178">從 `SE` 到 `TE`都有隱含的參考轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-178">An implicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="9cf32-179">從任何*array_type*到 `System.Array`，以及它所執行的介面。</span><span class="sxs-lookup"><span data-stu-id="9cf32-179">From any *array_type* to `System.Array` and the interfaces it implements.</span></span>
*  <span data-ttu-id="9cf32-180">從一維陣列類型 `S[]` 到 `System.Collections.Generic.IList<T>` 及其基底介面，但前提是從 `S` 到 `T`的隱含識別或參考轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-180">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an implicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="9cf32-181">從任何*delegate_type*到 `System.Delegate`，以及它所執行的介面。</span><span class="sxs-lookup"><span data-stu-id="9cf32-181">From any *delegate_type* to `System.Delegate` and the interfaces it implements.</span></span>
*  <span data-ttu-id="9cf32-182">從 null 常值到任何*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="9cf32-182">From the null literal to any *reference_type*.</span></span>
*  <span data-ttu-id="9cf32-183">從任何*reference_type*到*reference_type* `T` 如果它具有*reference_type* `T0` 的隱含識別或參考轉換，而且 `T0` 有身分識別轉換為 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-183">From any *reference_type* to a *reference_type* `T` if it has an implicit identity or reference conversion to a *reference_type* `T0` and `T0` has an identity conversion to `T`.</span></span>
*  <span data-ttu-id="9cf32-184">從任何*reference_type*到介面或委派類型 `T` 如果它具有介面或委派類型的隱含識別或參考轉換 `T0` 而且 `T0` 可轉換為 `T`的變異數（變異數[轉換](interfaces.md#variance-conversion)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-184">From any *reference_type* to an interface or delegate type `T` if it has an implicit identity or reference conversion to an interface or delegate type `T0` and `T0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `T`.</span></span>
*  <span data-ttu-id="9cf32-185">包含已知為參考型別之類型參數的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-185">Implicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="9cf32-186">如需有關涉及型別參數之隱含轉換的詳細資訊，請參閱[涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)。</span><span class="sxs-lookup"><span data-stu-id="9cf32-186">See [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) for more details on implicit conversions involving type parameters.</span></span>

<span data-ttu-id="9cf32-187">隱含參考轉換是*reference_type*之間的轉換，可證明一律成功，因此不需要在執行時間進行任何檢查。</span><span class="sxs-lookup"><span data-stu-id="9cf32-187">The implicit reference conversions are those conversions between *reference_type*s that can be proven to always succeed, and therefore require no checks at run-time.</span></span>

<span data-ttu-id="9cf32-188">參考轉換（隱含或明確）永遠不會變更正在轉換之物件的參考身分識別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-188">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="9cf32-189">換句話說，雖然參考轉換可能會變更參考的類型，但它不會變更所參考物件的類型或值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-189">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="9cf32-190">裝箱轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-190">Boxing conversions</span></span>

<span data-ttu-id="9cf32-191">「裝箱」轉換允許將*value_type*隱含地轉換成參考型別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-191">A boxing conversion permits a *value_type* to be implicitly converted to a reference type.</span></span> <span data-ttu-id="9cf32-192">從任何*non_nullable_value_type*到 `object` 和 `dynamic`的「裝箱」轉換，`System.ValueType` 和*interface_type*所實的任何*non_nullable_value_type* 。</span><span class="sxs-lookup"><span data-stu-id="9cf32-192">A boxing conversion exists from any *non_nullable_value_type* to `object` and `dynamic`, to `System.ValueType` and to any *interface_type* implemented by the *non_nullable_value_type*.</span></span> <span data-ttu-id="9cf32-193">此外， *enum_type*可以轉換成類型 `System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-193">Furthermore an *enum_type* can be converted to the type `System.Enum`.</span></span>

<span data-ttu-id="9cf32-194">只有在從基礎*non_nullable_value_type*到參考型別的裝箱轉換存在時，從*nullable_type*到參考型別的裝箱轉換才會存在。</span><span class="sxs-lookup"><span data-stu-id="9cf32-194">A boxing conversion exists from a *nullable_type* to a reference type, if and only if a boxing conversion exists from the underlying *non_nullable_value_type* to the reference type.</span></span>

<span data-ttu-id="9cf32-195">實值型別具有介面型別的「裝箱」轉換，`I` 如果它有 `I0` 的介面型別的裝箱轉換，而且 `I0` 具有 `I`的身分識別轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-195">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="9cf32-196">實值型別具有介面型別的裝箱轉換，`I` 如果它有介面或委派型別的裝箱轉換 `I0` 而且 `I0` 可以區分變異數（變異數[轉換](interfaces.md#variance-conversion)）為 `I`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-196">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface or delegate type `I0` and `I0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `I`.</span></span>

<span data-ttu-id="9cf32-197">為*non_nullable_value_type*的值進行裝箱，包括設定物件實例，以及將*value_type*值複製到該實例。</span><span class="sxs-lookup"><span data-stu-id="9cf32-197">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *value_type* value into that instance.</span></span> <span data-ttu-id="9cf32-198">結構可以封裝成型別 `System.ValueType`，因為這是所有結構（[繼承](structs.md#inheritance)）的基類（base class）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-198">A struct can be boxed to the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="9cf32-199">將*nullable_type*的值進行裝箱，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9cf32-199">Boxing a value of a *nullable_type* proceeds as follows:</span></span>

*  <span data-ttu-id="9cf32-200">如果來源值為 null （`HasValue` 屬性為 false），則結果會是目標型別的 null 參考。</span><span class="sxs-lookup"><span data-stu-id="9cf32-200">If the source value is null (`HasValue` property is false), the result is a null reference of the target type.</span></span>
*  <span data-ttu-id="9cf32-201">否則，結果會是透過解除包裝和裝箱來源值所產生的盒裝 `T` 的參考。</span><span class="sxs-lookup"><span data-stu-id="9cf32-201">Otherwise, the result is a reference to a boxed `T` produced by unwrapping and boxing the source value.</span></span>

<span data-ttu-id="9cf32-202">在「[裝箱」轉換](types.md#boxing-conversions)中會進一步說明「裝箱」轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-202">Boxing conversions are described further in [Boxing conversions](types.md#boxing-conversions).</span></span>

### <a name="implicit-dynamic-conversions"></a><span data-ttu-id="9cf32-203">隱含動態轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-203">Implicit dynamic conversions</span></span>

<span data-ttu-id="9cf32-204">從類型的運算式 `dynamic` 到任何類型 `T`的隱含動態轉換存在。</span><span class="sxs-lookup"><span data-stu-id="9cf32-204">An implicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="9cf32-205">轉換是動態系結的（[動態](expressions.md#dynamic-binding)系結），這表示在執行時間會從運算式的執行時間類型中搜尋隱含轉換，以 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-205">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an implicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="9cf32-206">如果找不到任何轉換，則會擲回執行時間例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9cf32-206">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="9cf32-207">請注意，這項隱含轉換似乎違反隱含轉換開始不會造成例外狀況的[隱性](conversions.md#implicit-conversions)轉換開頭的建議。</span><span class="sxs-lookup"><span data-stu-id="9cf32-207">Note that this implicit conversion seemingly violates the advice in the beginning of [Implicit conversions](conversions.md#implicit-conversions) that an implicit conversion should never cause an exception.</span></span> <span data-ttu-id="9cf32-208">不過，它不是轉換本身，而是*尋找*導致例外狀況的轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-208">However it is not the conversion itself, but the *finding* of the conversion that causes the exception.</span></span> <span data-ttu-id="9cf32-209">使用動態系結時，會造成執行時間例外狀況的風險。</span><span class="sxs-lookup"><span data-stu-id="9cf32-209">The risk of run-time exceptions is inherent in the use of dynamic binding.</span></span> <span data-ttu-id="9cf32-210">如果不想要轉換的動態系結，可以先將運算式轉換成 `object`，然後再轉換成所需的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-210">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="9cf32-211">下列範例說明隱含的動態轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-211">The following example illustrates implicit dynamic conversions:</span></span>

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

<span data-ttu-id="9cf32-212">`s2` 和 `i` 的指派會採用隱含的動態轉換，其中作業的系結會暫止到執行時間。</span><span class="sxs-lookup"><span data-stu-id="9cf32-212">The assignments to `s2` and `i` both employ implicit dynamic conversions, where the binding of the operations is suspended until run-time.</span></span> <span data-ttu-id="9cf32-213">在執行時間，隱含轉換會從執行時間類型的 `d` -- `string`--到目標型別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-213">At run-time, implicit conversions are sought from the run-time type of `d` -- `string` -- to the target type.</span></span> <span data-ttu-id="9cf32-214">`string` 發現轉換，而不是 `int`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-214">A conversion is found to `string` but not to `int`.</span></span>

### <a name="implicit-constant-expression-conversions"></a><span data-ttu-id="9cf32-215">隱含常數運算式轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-215">Implicit constant expression conversions</span></span>

<span data-ttu-id="9cf32-216">隱含的常數運算式轉換允許下列轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-216">An implicit constant expression conversion permits the following conversions:</span></span>

*  <span data-ttu-id="9cf32-217">如果 *`short`* 的值在目的地類型的範圍內，則 `int` 類型的*constant_expression* （[常數運算式](expressions.md#constant-expressions)）可以轉換成類型 `sbyte`、`byte`、`ushort`、`uint`、`ulong`或 constant_expression。</span><span class="sxs-lookup"><span data-stu-id="9cf32-217">A *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) of type `int` can be converted to type `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the *constant_expression* is within the range of the destination type.</span></span>
*  <span data-ttu-id="9cf32-218">如果*constant_expression*的值不是負值，則 `long` 類型的*constant_expression*可以轉換成類型 `ulong`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-218">A *constant_expression* of type `long` can be converted to type `ulong`, provided the value of the *constant_expression* is not negative.</span></span>

### <a name="implicit-conversions-involving-type-parameters"></a><span data-ttu-id="9cf32-219">涉及型別參數的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-219">Implicit conversions involving type parameters</span></span>

<span data-ttu-id="9cf32-220">指定的類型參數 `T`有下列隱含轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-220">The following implicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="9cf32-221">從 `T` 到其有效基類 `C`，從 `T` 到 `C`的任何基類，以及從 `T` 到 `C`所實的任何介面。</span><span class="sxs-lookup"><span data-stu-id="9cf32-221">From `T` to its effective base class `C`, from `T` to any base class of `C`, and from `T` to any interface implemented by `C`.</span></span> <span data-ttu-id="9cf32-222">在執行時間，如果 `T` 是實值型別，則會以「裝箱」轉換來執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-222">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="9cf32-223">否則，會以隱含的參考轉換或身分識別轉換來執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-223">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="9cf32-224">從 `T` 到 `T`有效介面集內 `I` 介面類別型，以及從 `T` 到 `I`的任何基底介面。</span><span class="sxs-lookup"><span data-stu-id="9cf32-224">From `T` to an interface type `I` in `T`'s effective interface set and from `T` to any base interface of `I`.</span></span> <span data-ttu-id="9cf32-225">在執行時間，如果 `T` 是實值型別，則會以「裝箱」轉換來執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-225">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="9cf32-226">否則，會以隱含的參考轉換或身分識別轉換來執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-226">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="9cf32-227">從 `T` 到型別參數 `U`，提供 `T` 取決於 `U` （[型別參數條件約束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-227">From `T` to a type parameter `U`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="9cf32-228">在執行時間，如果 `U` 是實值型別，則 `T` 和 `U` 一定是相同的型別，而且不會執行任何轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-228">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="9cf32-229">否則，如果 `T` 是實值型別，則會以「裝箱」轉換來執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-229">Otherwise, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="9cf32-230">否則，會以隱含的參考轉換或身分識別轉換來執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-230">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="9cf32-231">從 null 常值到 `T`，提供的 `T` 已知為參考型別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-231">From the null literal to `T`, provided `T` is known to be a reference type.</span></span>
*  <span data-ttu-id="9cf32-232">從 `T` 到參考型別 `I` 如果它有隱含轉換成參考型別 `S0` 而且 `S0` 有識別轉換成 `S`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-232">From `T` to a reference type `I` if it has an implicit conversion to a reference type `S0` and `S0` has an identity conversion to `S`.</span></span> <span data-ttu-id="9cf32-233">在執行時間，轉換的執行方式與轉換 `S0`相同。</span><span class="sxs-lookup"><span data-stu-id="9cf32-233">At run-time the conversion is executed the same way as the conversion to `S0`.</span></span>
*  <span data-ttu-id="9cf32-234">從 `T` 到介面型別，`I` 如果它具有介面或委派型別的隱含轉換 `I0` 而且 `I0` 可轉換成 `I` （變異數[轉換](interfaces.md#variance-conversion)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-234">From `T` to an interface type `I` if it has an implicit conversion to an interface or delegate type `I0` and `I0` is variance-convertible to `I` ([Variance conversion](interfaces.md#variance-conversion)).</span></span> <span data-ttu-id="9cf32-235">在執行時間，如果 `T` 是實值型別，則會以「裝箱」轉換來執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-235">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="9cf32-236">否則，會以隱含的參考轉換或身分識別轉換來執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-236">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="9cf32-237">如果已知 `T` 是參考型別（型別[參數條件約束](classes.md#type-parameter-constraints)），上述的轉換就會分類為隱含參考轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-237">If `T` is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)), the conversions above are all classified as implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="9cf32-238">如果 `T` 不知道是參考型別，上述的轉換會分類為「裝箱」轉換（「[裝箱](conversions.md#boxing-conversions)」轉換）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-238">If `T` is not known to be a reference type, the conversions above are classified as boxing conversions ([Boxing conversions](conversions.md#boxing-conversions)).</span></span>

### <a name="user-defined-implicit-conversions"></a><span data-ttu-id="9cf32-239">使用者定義的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-239">User-defined implicit conversions</span></span>

<span data-ttu-id="9cf32-240">使用者定義的隱含轉換是由選擇性的標準隱含轉換所組成，接著執行使用者定義的隱含轉換運算子，再接著另一個選擇性的標準隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-240">A user-defined implicit conversion consists of an optional standard implicit conversion, followed by execution of a user-defined implicit conversion operator, followed by another optional standard implicit conversion.</span></span> <span data-ttu-id="9cf32-241">在[處理使用者定義的隱含](conversions.md#processing-of-user-defined-implicit-conversions)轉換時，會說明評估使用者定義隱含轉換的確切規則。</span><span class="sxs-lookup"><span data-stu-id="9cf32-241">The exact rules for evaluating user-defined implicit conversions are described in [Processing of user-defined implicit conversions](conversions.md#processing-of-user-defined-implicit-conversions).</span></span>

### <a name="anonymous-function-conversions-and-method-group-conversions"></a><span data-ttu-id="9cf32-242">匿名函數轉換和方法群組轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-242">Anonymous function conversions and method group conversions</span></span>

<span data-ttu-id="9cf32-243">匿名函式和方法群組本身並沒有類型，但可以隱含地轉換成委派類型或運算式樹狀架構類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-243">Anonymous functions and method groups do not have types in and of themselves, but may be implicitly converted to delegate types or expression tree types.</span></span> <span data-ttu-id="9cf32-244">匿名函數轉換在[方法群組轉換](conversions.md#method-group-conversions)中的[匿名](conversions.md#anonymous-function-conversions)函式轉換和方法群組轉換中有更詳細的說明。</span><span class="sxs-lookup"><span data-stu-id="9cf32-244">Anonymous function conversions are described in more detail in [Anonymous function conversions](conversions.md#anonymous-function-conversions) and method group conversions in [Method group conversions](conversions.md#method-group-conversions).</span></span>

## <a name="explicit-conversions"></a><span data-ttu-id="9cf32-245">明確轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-245">Explicit conversions</span></span>

<span data-ttu-id="9cf32-246">下列轉換會分類為明確轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-246">The following conversions are classified as explicit conversions:</span></span>

*  <span data-ttu-id="9cf32-247">所有隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-247">All implicit conversions.</span></span>
*  <span data-ttu-id="9cf32-248">明確數值轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-248">Explicit numeric conversions.</span></span>
*  <span data-ttu-id="9cf32-249">明確列舉轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-249">Explicit enumeration conversions.</span></span>
*  <span data-ttu-id="9cf32-250">可為 null 的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-250">Explicit nullable conversions.</span></span>
*  <span data-ttu-id="9cf32-251">明確參考轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-251">Explicit reference conversions.</span></span>
*  <span data-ttu-id="9cf32-252">明確的介面轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-252">Explicit interface conversions.</span></span>
*  <span data-ttu-id="9cf32-253">取消裝箱轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-253">Unboxing conversions.</span></span>
*  <span data-ttu-id="9cf32-254">明確動態轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-254">Explicit dynamic conversions</span></span>
*  <span data-ttu-id="9cf32-255">使用者定義的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-255">User-defined explicit conversions.</span></span>

<span data-ttu-id="9cf32-256">明確轉換可以在 cast 運算式中進行（[cast 運算式](expressions.md#cast-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-256">Explicit conversions can occur in cast expressions ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="9cf32-257">明確轉換的集合包含所有隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-257">The set of explicit conversions includes all implicit conversions.</span></span> <span data-ttu-id="9cf32-258">這表示允許使用多餘的轉換運算式。</span><span class="sxs-lookup"><span data-stu-id="9cf32-258">This means that redundant cast expressions are allowed.</span></span>

<span data-ttu-id="9cf32-259">不是隱含轉換的明確轉換是無法證明一律成功的轉換、已知可能會遺失資訊的轉換，以及跨類型的多個網域的轉換，足以明確地進行萬用字元.</span><span class="sxs-lookup"><span data-stu-id="9cf32-259">The explicit conversions that are not implicit conversions are conversions that cannot be proven to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit explicit notation.</span></span>

### <a name="explicit-numeric-conversions"></a><span data-ttu-id="9cf32-260">明確數值轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-260">Explicit numeric conversions</span></span>

<span data-ttu-id="9cf32-261">明確數值轉換是從*numeric_type*到另一個*numeric_type*的轉換，也就是隱含數值轉換（[隱含數值](conversions.md#implicit-numeric-conversions)轉換）尚不存在：</span><span class="sxs-lookup"><span data-stu-id="9cf32-261">The explicit numeric conversions are the conversions from a *numeric_type* to another *numeric_type* for which an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) does not already exist:</span></span>

*  <span data-ttu-id="9cf32-262">從 `sbyte` 到 `byte`、`ushort`、`uint`、`ulong`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-262">From `sbyte` to `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="9cf32-263">從 `byte` 到 `sbyte` 並 `char`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-263">From `byte` to `sbyte` and `char`.</span></span>
*  <span data-ttu-id="9cf32-264">從 `short` 到 `sbyte`、`byte`、`ushort`、`uint`、`ulong`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-264">From `short` to `sbyte`, `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="9cf32-265">從 `ushort` 到 `sbyte`、`byte`、`short`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-265">From `ushort` to `sbyte`, `byte`, `short`, or `char`.</span></span>
*  <span data-ttu-id="9cf32-266">從 `int` 到 `sbyte`、`byte`、`short`、`ushort`、`uint`、`ulong`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-266">From `int` to `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="9cf32-267">從 `uint` 到 `sbyte`、`byte`、`short`、`ushort`、`int`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-267">From `uint` to `sbyte`, `byte`, `short`, `ushort`, `int`, or `char`.</span></span>
*  <span data-ttu-id="9cf32-268">從 `long` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`ulong`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-268">From `long` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="9cf32-269">從 `ulong` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-269">From `ulong` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `char`.</span></span>
*  <span data-ttu-id="9cf32-270">從 `char` 到 `sbyte`、`byte`或 `short`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-270">From `char` to `sbyte`, `byte`, or `short`.</span></span>
*  <span data-ttu-id="9cf32-271">從 `float` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-271">From `float` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-272">從 `double` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-272">From `double` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-273">從 `decimal` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`或 `double`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-273">From `decimal` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `double`.</span></span>

<span data-ttu-id="9cf32-274">由於明確轉換包括所有隱含和明確的數值轉換，因此一律可以使用 cast 運算式（[cast 運算式](expressions.md#cast-expressions)），從任何*numeric_type*轉換成任何其他*numeric_type* 。</span><span class="sxs-lookup"><span data-stu-id="9cf32-274">Because the explicit conversions include all implicit and explicit numeric conversions, it is always possible to convert from any *numeric_type* to any other *numeric_type* using a cast expression ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="9cf32-275">明確的數值轉換可能會遺失資訊，或可能導致擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9cf32-275">The explicit numeric conversions possibly lose information or possibly cause exceptions to be thrown.</span></span> <span data-ttu-id="9cf32-276">明確數值轉換的處理方式如下：</span><span class="sxs-lookup"><span data-stu-id="9cf32-276">An explicit numeric conversion is processed as follows:</span></span>

*  <span data-ttu-id="9cf32-277">若要從整數類資料類型轉換為另一個整數類型，處理方式取決於發生轉換的溢位檢查內容（[checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)）：</span><span class="sxs-lookup"><span data-stu-id="9cf32-277">For a conversion from an integral type to another integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="9cf32-278">在 `checked` 內容中，如果來源運算元的值在目的地類型的範圍內，轉換就會成功，但如果來源運算元的值超出目的地類型的範圍，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-278">In a `checked` context, the conversion succeeds if the value of the source operand is within the range of the destination type, but throws a `System.OverflowException` if the value of the source operand is outside the range of the destination type.</span></span>
    * <span data-ttu-id="9cf32-279">在 `unchecked` 內容中，轉換一律會成功，並依照下列方式繼續進行。</span><span class="sxs-lookup"><span data-stu-id="9cf32-279">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="9cf32-280">如果來源類型大於目的地類型，會藉由捨棄其「額外」最高有效位元，來截斷來源值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-280">If the source type is larger than the destination type, then the source value is truncated by discarding its "extra" most significant bits.</span></span> <span data-ttu-id="9cf32-281">然後會將結果視為目標類型的值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-281">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="9cf32-282">如果來源類型小於目標類型，則來源值可以是正負號擴充或零擴充，以使其與目標類型的大小相同。</span><span class="sxs-lookup"><span data-stu-id="9cf32-282">If the source type is smaller than the destination type, then the source value is either sign-extended or zero-extended so that it is the same size as the destination type.</span></span> <span data-ttu-id="9cf32-283">如果來源類型帶正負號，則會使用正負號擴充；如果來源類型不帶正負號，則會使用零擴充。</span><span class="sxs-lookup"><span data-stu-id="9cf32-283">Sign-extension is used if the source type is signed; zero-extension is used if the source type is unsigned.</span></span> <span data-ttu-id="9cf32-284">然後會將結果視為目標類型的值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-284">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="9cf32-285">如果來源類型與目標類型的大小相同，則來源值將視為目標類型的值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-285">If the source type is the same size as the destination type, then the source value is treated as a value of the destination type.</span></span>
*  <span data-ttu-id="9cf32-286">從 `decimal` 轉換成整數類資料類型時，會將來源值舍入至最接近整數值的零，而這個整數值會變成轉換的結果。</span><span class="sxs-lookup"><span data-stu-id="9cf32-286">For a conversion from `decimal` to an integral type, the source value is rounded towards zero to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="9cf32-287">如果產生的整數值超出目的地類型的範圍，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-287">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="9cf32-288">若要從 `float` 或 `double` 轉換成整數類資料類型，處理會根據發生轉換的溢位檢查內容（[checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)）而定：</span><span class="sxs-lookup"><span data-stu-id="9cf32-288">For a conversion from `float` or `double` to an integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="9cf32-289">在 `checked` 內容中，轉換會繼續進行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9cf32-289">In a `checked` context, the conversion proceeds as follows:</span></span>
        * <span data-ttu-id="9cf32-290">如果運算元的值是 NaN 或無限大，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-290">If the value of the operand is NaN or infinite, a `System.OverflowException` is thrown.</span></span>
        * <span data-ttu-id="9cf32-291">否則，會將來源運算元向零進位到最接近的整數值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-291">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="9cf32-292">如果這個整數值在目的地類型的範圍內，則這個值是轉換的結果。</span><span class="sxs-lookup"><span data-stu-id="9cf32-292">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="9cf32-293">否則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-293">Otherwise, a `System.OverflowException` is thrown.</span></span>
    * <span data-ttu-id="9cf32-294">在 `unchecked` 內容中，轉換一律會成功，並依照下列方式繼續進行。</span><span class="sxs-lookup"><span data-stu-id="9cf32-294">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="9cf32-295">如果運算元的值是 NaN 或無限大，則轉換的結果會是目的地類型的未指定值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-295">If the value of the operand is NaN or infinite, the result of the conversion is an unspecified value of the destination type.</span></span>
        * <span data-ttu-id="9cf32-296">否則，會將來源運算元向零進位到最接近的整數值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-296">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="9cf32-297">如果這個整數值在目的地類型的範圍內，則這個值是轉換的結果。</span><span class="sxs-lookup"><span data-stu-id="9cf32-297">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="9cf32-298">否則，轉換的結果會是目的地類型的未指定值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-298">Otherwise, the result of the conversion is an unspecified value of the destination type.</span></span>
*  <span data-ttu-id="9cf32-299">若要從 `double` 轉換為 `float`，`double` 值會四捨五入為最接近的 `float` 值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-299">For a conversion from `double` to `float`, the `double` value is rounded to the nearest `float` value.</span></span> <span data-ttu-id="9cf32-300">如果 `double` 值太小而無法表示為 `float`，則結果會變成正零或負零。</span><span class="sxs-lookup"><span data-stu-id="9cf32-300">If the `double` value is too small to represent as a `float`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="9cf32-301">如果 `double` 值太大，而無法表示為 `float`，則結果會變成正無限大或負無限大。</span><span class="sxs-lookup"><span data-stu-id="9cf32-301">If the `double` value is too large to represent as a `float`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="9cf32-302">如果 `double` 值是 NaN，則結果也是 NaN。</span><span class="sxs-lookup"><span data-stu-id="9cf32-302">If the `double` value is NaN, the result is also NaN.</span></span>
*  <span data-ttu-id="9cf32-303">若要從 `float` 或 `double` 轉換為 `decimal`，會將來源值轉換成 `decimal` 表示，並在必要時將其四捨五入為最接近的數位（[decimal 類型](types.md#the-decimal-type)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-303">For a conversion from `float` or `double` to `decimal`, the source value is converted to `decimal` representation and rounded to the nearest number after the 28th decimal place if required ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="9cf32-304">如果來源值太小而無法表示為 `decimal`，則結果會變成零。</span><span class="sxs-lookup"><span data-stu-id="9cf32-304">If the source value is too small to represent as a `decimal`, the result becomes zero.</span></span> <span data-ttu-id="9cf32-305">如果來源值為 NaN、無限大，或太大而無法表示為 `decimal`，就會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-305">If the source value is NaN, infinity, or too large to represent as a `decimal`, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="9cf32-306">若要從 `decimal` 到 `float` 或 `double`的轉換，`decimal` 值會舍入到最接近的 `double` 或 `float` 值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-306">For a conversion from `decimal` to `float` or `double`, the `decimal` value is rounded to the nearest `double` or `float` value.</span></span> <span data-ttu-id="9cf32-307">雖然這種轉換可能會遺失精確度，但不會導致擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9cf32-307">While this conversion may lose precision, it never causes an exception to be thrown.</span></span>

### <a name="explicit-enumeration-conversions"></a><span data-ttu-id="9cf32-308">明確列舉轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-308">Explicit enumeration conversions</span></span>

<span data-ttu-id="9cf32-309">明確列舉轉換如下：</span><span class="sxs-lookup"><span data-stu-id="9cf32-309">The explicit enumeration conversions are:</span></span>

*  <span data-ttu-id="9cf32-310">從 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`或 `decimal` 到任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="9cf32-310">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal` to any *enum_type*.</span></span>
*  <span data-ttu-id="9cf32-311">從任何*enum_type*到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-311">From any *enum_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="9cf32-312">從任何*enum_type*到任何其他*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="9cf32-312">From any *enum_type* to any other *enum_type*.</span></span>

<span data-ttu-id="9cf32-313">在兩個類型之間進行明確列舉轉換的處理方式是將任何參與的*enum_type*視為該*enum_type*的基礎類型，然後在產生的類型之間執行隱含或明確的數值轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-313">An explicit enumeration conversion between two types is processed by treating any participating *enum_type* as the underlying type of that *enum_type*, and then performing an implicit or explicit numeric conversion between the resulting types.</span></span> <span data-ttu-id="9cf32-314">例如，假設*enum_type* `E` 和 `int`的基礎類型，則 `E` 從 `byte` 到 `int` 的轉換會當做明確的數值轉換（明確的[數值](conversions.md#explicit-numeric-conversions)轉換）來處理，並從 `byte`轉換成 `byte`，然後將從 `E` 到 `byte` 的轉換當做隱含數值轉換（[隱含數值](conversions.md#implicit-numeric-conversions)轉換），從 `int`到進行處理。</span><span class="sxs-lookup"><span data-stu-id="9cf32-314">For example, given an *enum_type* `E` with and underlying type of `int`, a conversion from `E` to `byte` is processed as an explicit numeric conversion ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from `int` to `byte`, and a conversion from `byte` to `E` is processed as an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) from `byte` to `int`.</span></span>

### <a name="explicit-nullable-conversions"></a><span data-ttu-id="9cf32-315">明確可為 null 的轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-315">Explicit nullable conversions</span></span>

<span data-ttu-id="9cf32-316">***明確的可為 null 轉換***允許在不可為 null 的實數值型別上運作的預先定義明確轉換，也可以搭配這些類型的可為 null 形式來使用。</span><span class="sxs-lookup"><span data-stu-id="9cf32-316">***Explicit nullable conversions*** permit predefined explicit conversions that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="9cf32-317">對於從不可為 null 的實值型別轉換 `S` 為不可為 null 的實值型別 `T` （[識別轉換](conversions.md#identity-conversion)、[隱含數值轉換](conversions.md#implicit-numeric-conversions)、[隱含列舉轉換](conversions.md#implicit-enumeration-conversions)、[明確的數值轉換](conversions.md#explicit-numeric-conversions)和[明確列舉轉換](conversions.md#explicit-enumeration-conversions)）的每個預先定義的明確轉換，都存在下列可為 null 的轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-317">For each of the predefined explicit conversions that convert from a non-nullable value type `S` to a non-nullable value type `T` ([Identity conversion](conversions.md#identity-conversion), [Implicit numeric conversions](conversions.md#implicit-numeric-conversions), [Implicit enumeration conversions](conversions.md#implicit-enumeration-conversions), [Explicit numeric conversions](conversions.md#explicit-numeric-conversions), and [Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)), the following nullable conversions exist:</span></span>

*  <span data-ttu-id="9cf32-318">從 `S?` 到 `T?`的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-318">An explicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="9cf32-319">從 `S` 到 `T?`的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-319">An explicit conversion from `S` to `T?`.</span></span>
*  <span data-ttu-id="9cf32-320">從 `S?` 到 `T`的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-320">An explicit conversion from `S?` to `T`.</span></span>

<span data-ttu-id="9cf32-321">根據從 `S` 到 `T` 的基礎轉換，評估可為 null 的轉換，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9cf32-321">Evaluation of a nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="9cf32-322">如果可為 null 的轉換是從 `S?` 到 `T?`：</span><span class="sxs-lookup"><span data-stu-id="9cf32-322">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="9cf32-323">如果來源值為 null （`HasValue` 屬性為 false），則結果會是 `T?`類型的 null 值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-323">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="9cf32-324">否則，會將轉換評估為從 `S?` 解除包裝為 `S`，然後從 `S` 到 `T`進行基礎轉換，然後再從 `T` 換成 `T?`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-324">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="9cf32-325">如果可為 null 的轉換是從 `S` 到 `T?`，則會將轉換評估為從 `S` 到 `T` 的基礎轉換，然後再從 `T` 換成 `T?`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-325">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="9cf32-326">如果可為 null 的轉換是從 `S?` 到 `T`，則會將轉換評估為從 `S?` 解除包裝到 `S`，然後再從 `S` 到 `T`進行基礎轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-326">If the nullable conversion is from `S?` to `T`, the conversion is evaluated as an unwrapping from `S?` to `S` followed by the underlying conversion from `S` to `T`.</span></span>

<span data-ttu-id="9cf32-327">請注意，如果 `null`值，則嘗試解除包裝可為 null 的值將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9cf32-327">Note that an attempt to unwrap a nullable value will throw an exception if the value is `null`.</span></span>

### <a name="explicit-reference-conversions"></a><span data-ttu-id="9cf32-328">明確參考轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-328">Explicit reference conversions</span></span>

<span data-ttu-id="9cf32-329">明確的參考轉換包括：</span><span class="sxs-lookup"><span data-stu-id="9cf32-329">The explicit reference conversions are:</span></span>

*  <span data-ttu-id="9cf32-330">從 `object` 和 `dynamic` 到任何其他*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="9cf32-330">From `object` and `dynamic` to any other *reference_type*.</span></span>
*  <span data-ttu-id="9cf32-331">從任何*class_type* `S` 到任何*class_type*的 `T`，提供 `S` 是 `T`的基類。</span><span class="sxs-lookup"><span data-stu-id="9cf32-331">From any *class_type* `S` to any *class_type* `T`, provided `S` is a base class of `T`.</span></span>
*  <span data-ttu-id="9cf32-332">從任何*class_type* `S` 到任何*interface_type*的 `T`，提供的 `S` 不是密封的，而且 `S` 不會執行 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-332">From any *class_type* `S` to any *interface_type* `T`, provided `S` is not sealed and provided `S` does not implement `T`.</span></span>
*  <span data-ttu-id="9cf32-333">從任何*interface_type* `S` 到任何*class_type*的 `T`，提供 `T` 不是密封或提供 `T` `S`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-333">From any *interface_type* `S` to any *class_type* `T`, provided `T` is not sealed or provided `T` implements `S`.</span></span>
*  <span data-ttu-id="9cf32-334">從任何*interface_type* `S` 到任何*interface_type* `T`，提供的 `S` 不是衍生自 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-334">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is not derived from `T`.</span></span>
*  <span data-ttu-id="9cf32-335">從具有元素類型的*array_type* `S` `SE` 至專案類型 `T` 的*array_type* `TE`，前提是下列所有條件皆成立：</span><span class="sxs-lookup"><span data-stu-id="9cf32-335">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="9cf32-336">`S` 和 `T` 只有元素類型不同。</span><span class="sxs-lookup"><span data-stu-id="9cf32-336">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="9cf32-337">換句話說，`S` 和 `T` 的維度數目相同。</span><span class="sxs-lookup"><span data-stu-id="9cf32-337">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="9cf32-338">`SE` 和 `TE` 都是*reference_type*s。</span><span class="sxs-lookup"><span data-stu-id="9cf32-338">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="9cf32-339">從 `SE` 到 `TE`都有明確的參考轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-339">An explicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="9cf32-340">從 `System.Array` 和它所執行的介面到任何*array_type*。</span><span class="sxs-lookup"><span data-stu-id="9cf32-340">From `System.Array` and the interfaces it implements to any *array_type*.</span></span>
*  <span data-ttu-id="9cf32-341">從一維陣列類型 `S[]` 到 `System.Collections.Generic.IList<T>` 及其基底介面，但前提是從 `S` 到 `T`的明確參考轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-341">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an explicit reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="9cf32-342">從 `System.Collections.Generic.IList<S>` 及其基底介面到 `T[]`的一維陣列類型，前提是有明確識別或從 `S` 到 `T`的參考轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-342">From `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]`, provided that there is an explicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="9cf32-343">從 `System.Delegate` 和它所執行的介面到任何*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="9cf32-343">From `System.Delegate` and the interfaces it implements to any *delegate_type*.</span></span>
*  <span data-ttu-id="9cf32-344">從參考型別到參考型別 `T` 如果它有參考型別的明確參考轉換 `T0` 而且 `T0` 具有 `T`的識別轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-344">From a reference type to a reference type `T` if it has an explicit reference conversion to a reference type `T0` and `T0` has an identity conversion `T`.</span></span>
*  <span data-ttu-id="9cf32-345">從參考型別到介面或委派型別 `T` 如果它有介面或委派型別的明確參考轉換 `T0` 而且 `T0` 可以轉換成 `T` 或 `T` 可轉換成 `T0` （變異數[轉換](interfaces.md#variance-conversion)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-345">From a reference type to an interface or delegate type `T` if it has an explicit reference conversion to an interface or delegate type `T0` and either `T0` is variance-convertible to `T` or `T` is variance-convertible to `T0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>
*  <span data-ttu-id="9cf32-346">從 `D<S1...Sn>` 到 `D<T1...Tn>`，其中 `D<X1...Xn>` 是泛型委派型別，`D<S1...Sn>` 與 `D<T1...Tn>`不相容，而針對每個型別參數 `Xi` `D` 下列各項：</span><span class="sxs-lookup"><span data-stu-id="9cf32-346">From `D<S1...Sn>` to `D<T1...Tn>` where `D<X1...Xn>` is a generic delegate type, `D<S1...Sn>` is not compatible with or identical to `D<T1...Tn>`, and for each type parameter `Xi` of `D` the following holds:</span></span>
    * <span data-ttu-id="9cf32-347">如果 `Xi` 是不變的，則 `Si` 等同于 `Ti`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-347">If `Xi` is invariant, then `Si` is identical to `Ti`.</span></span>
    * <span data-ttu-id="9cf32-348">如果 `Xi` 是協變數的，則會有從 `Si` 到 `Ti`的隱含或明確識別或參考轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-348">If `Xi` is covariant, then there is an implicit or explicit identity or reference conversion from `Si` to `Ti`.</span></span>
    * <span data-ttu-id="9cf32-349">如果 `Xi` 為逆變性，則 `Si` 和 `Ti` 都是相同或兩個參考型別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-349">If `Xi` is contravariant, then `Si` and `Ti` are either identical or both reference types.</span></span>
*  <span data-ttu-id="9cf32-350">包含已知為參考型別之類型參數的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-350">Explicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="9cf32-351">如需有關涉及型別參數之明確轉換的詳細資訊，請參閱[涉及型別參數的明確轉換](conversions.md#explicit-conversions-involving-type-parameters)。</span><span class="sxs-lookup"><span data-stu-id="9cf32-351">For more details on explicit conversions involving type parameters, see [Explicit conversions involving type parameters](conversions.md#explicit-conversions-involving-type-parameters).</span></span>

<span data-ttu-id="9cf32-352">明確參考轉換是指需要執行時間檢查以確保它們正確的參考型別之間的轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-352">The explicit reference conversions are those conversions between reference-types that require run-time checks to ensure they are correct.</span></span>

<span data-ttu-id="9cf32-353">若要在執行時間成功進行明確的參考轉換，必須 `null`來源運算元的值，或來源運算元所參考之物件的實際類型，必須是可以透過隱含參考轉換（[隱含參考](conversions.md#implicit-reference-conversions)轉換）或「裝箱轉換」（「[裝箱](conversions.md#boxing-conversions)轉換」）轉換成目的類型的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-353">For an explicit reference conversion to succeed at run-time, the value of the source operand must be `null`, or the actual type of the object referenced by the source operand must be a type that can be converted to the destination type by an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)).</span></span> <span data-ttu-id="9cf32-354">如果明確的參考轉換失敗，則會擲回 `System.InvalidCastException`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-354">If an explicit reference conversion fails, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="9cf32-355">參考轉換（隱含或明確）永遠不會變更正在轉換之物件的參考身分識別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-355">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="9cf32-356">換句話說，雖然參考轉換可能會變更參考的類型，但它不會變更所參考物件的類型或值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-356">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="9cf32-357">取消裝箱轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-357">Unboxing conversions</span></span>

<span data-ttu-id="9cf32-358">「取消裝箱」轉換允許將參考型別明確轉換成*value_type*。</span><span class="sxs-lookup"><span data-stu-id="9cf32-358">An unboxing conversion permits a reference type to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="9cf32-359">從 `object`的類型、`dynamic` 和 `System.ValueType` 到任何*non_nullable_value_type*，以及從任何*interface_type*到任何會執行*non_nullable_value_type*的任何*interface_type* ，都有一個取消程式轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-359">An unboxing conversion exists from the types `object`, `dynamic` and `System.ValueType` to any *non_nullable_value_type*, and from any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span> <span data-ttu-id="9cf32-360">此外，您也可以將類型 `System.Enum` 取消加入任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="9cf32-360">Furthermore type `System.Enum` can be unboxed to any *enum_type*.</span></span>

<span data-ttu-id="9cf32-361">如果從參考型別存在到*nullable_type*的基礎*non_nullable_value_type*的取消裝箱轉換，則會從參考型別到*nullable_type*的取消裝箱轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-361">An unboxing conversion exists from a reference type to a *nullable_type* if an unboxing conversion exists from the reference type to the underlying *non_nullable_value_type* of the *nullable_type*.</span></span>

<span data-ttu-id="9cf32-362">實值型別 `S` 具有從介面型別進行的取消裝箱轉換 `I` 如果它有從介面型別進行的取消裝箱轉換 `I0` 而且 `I0` 會將身分識別轉換成 `I`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-362">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="9cf32-363">實值型別 `S` 具有從介面型別進行的取消裝箱轉換 `I` 如果它有從介面或委派型別進行的取消裝箱轉換 `I0` 而且 `I0` 可轉換成 `I` 或 `I` 可以轉換成 `I0` （變異數[轉換](interfaces.md#variance-conversion)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-363">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface or delegate type `I0` and either `I0` is variance-convertible to `I` or `I` is variance-convertible to `I0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>

<span data-ttu-id="9cf32-364">取消封裝作業包含第一次檢查物件實例是否為指定*value_type*的已裝箱值，然後從實例複製值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-364">An unboxing operation consists of first checking that the object instance is a boxed value of the given *value_type*, and then copying the value out of the instance.</span></span> <span data-ttu-id="9cf32-365">取消對*nullable_type*的 null 參考時，會產生*nullable_type*的 null 值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-365">Unboxing a null reference to a *nullable_type* produces the null value of the *nullable_type*.</span></span> <span data-ttu-id="9cf32-366">結構可以從類型 `System.ValueType`中取消裝箱，因為這是所有結構（[繼承](structs.md#inheritance)）的基類。</span><span class="sxs-lookup"><span data-stu-id="9cf32-366">A struct can be unboxed from the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="9cf32-367">取消[裝箱轉換中會](types.md#unboxing-conversions)進一步說明取消功能轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-367">Unboxing conversions are described further in [Unboxing conversions](types.md#unboxing-conversions).</span></span>

### <a name="explicit-dynamic-conversions"></a><span data-ttu-id="9cf32-368">明確動態轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-368">Explicit dynamic conversions</span></span>

<span data-ttu-id="9cf32-369">從類型的運算式 `dynamic` 到任何類型 `T`都有明確的動態轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-369">An explicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="9cf32-370">轉換是動態系結的（[動態](expressions.md#dynamic-binding)系結），這表示在執行時間會從運算式的執行時間類型中搜尋明確轉換，以 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-370">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an explicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="9cf32-371">如果找不到任何轉換，則會擲回執行時間例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9cf32-371">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="9cf32-372">如果不想要轉換的動態系結，可以先將運算式轉換成 `object`，然後再轉換成所需的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-372">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="9cf32-373">假設已定義下列類別：</span><span class="sxs-lookup"><span data-stu-id="9cf32-373">Assume the following class is defined:</span></span>
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

<span data-ttu-id="9cf32-374">下列範例說明明確的動態轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-374">The following example illustrates explicit dynamic conversions:</span></span>
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

<span data-ttu-id="9cf32-375">在編譯時期，將 `o` 到 `C` 的最佳轉換是明確的參考轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-375">The best conversion of `o` to `C` is found at compile-time to be an explicit reference conversion.</span></span> <span data-ttu-id="9cf32-376">這會在執行時間失敗，因為 `"1"` 事實上並不是 `C`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-376">This fails at run-time, because `"1"` is not in fact a `C`.</span></span> <span data-ttu-id="9cf32-377">不過，將 `d` 轉換成 `C` 明確動態轉換的執行時間，會將使用者定義從執行時間類型的 `d` -- `string`--轉換成 `C`），並成功完成。</span><span class="sxs-lookup"><span data-stu-id="9cf32-377">The conversion of `d` to `C` however, as an explicit dynamic conversion, is suspended to run-time, where a user defined conversion from the run-time type of `d` -- `string` -- to `C` is found, and succeeds.</span></span>

### <a name="explicit-conversions-involving-type-parameters"></a><span data-ttu-id="9cf32-378">涉及型別參數的明確轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-378">Explicit conversions involving type parameters</span></span>

<span data-ttu-id="9cf32-379">下列明確轉換存在於指定的類型參數 `T`：</span><span class="sxs-lookup"><span data-stu-id="9cf32-379">The following explicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="9cf32-380">從有效的基類 `C` `T` 到 `T`，以及從任何 `C` 基類到 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-380">From the effective base class `C` of `T` to `T` and from any base class of `C` to `T`.</span></span> <span data-ttu-id="9cf32-381">在執行時間，如果 `T` 是實值型別，則會以「取消裝箱」轉換來執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-381">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="9cf32-382">否則，轉換會當做明確參考轉換或身分識別轉換來執行。</span><span class="sxs-lookup"><span data-stu-id="9cf32-382">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="9cf32-383">從任何介面類別型到 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-383">From any interface type to `T`.</span></span> <span data-ttu-id="9cf32-384">在執行時間，如果 `T` 是實值型別，則會以「取消裝箱」轉換來執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-384">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="9cf32-385">否則，轉換會當做明確參考轉換或身分識別轉換來執行。</span><span class="sxs-lookup"><span data-stu-id="9cf32-385">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="9cf32-386">從 `T` 到任何*interface_type* `I` 提供的不是從 `T` 到 `I`的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-386">From `T` to any *interface_type* `I` provided there is not already an implicit conversion from `T` to `I`.</span></span> <span data-ttu-id="9cf32-387">在執行時間，如果 `T` 是實值型別，則轉換會執行為裝箱轉換，後面接著明確的參考轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-387">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion followed by an explicit reference conversion.</span></span> <span data-ttu-id="9cf32-388">否則，轉換會當做明確參考轉換或身分識別轉換來執行。</span><span class="sxs-lookup"><span data-stu-id="9cf32-388">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="9cf32-389">從型別參數 `U` 至 `T`，但前提 `T` 取決於 `U` （[型別參數條件約束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-389">From a type parameter `U` to `T`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="9cf32-390">在執行時間，如果 `U` 是實值型別，則 `T` 和 `U` 一定是相同的型別，而且不會執行任何轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-390">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="9cf32-391">否則，如果 `T` 是實值型別，則會將轉換當做取消裝箱轉換來執行。</span><span class="sxs-lookup"><span data-stu-id="9cf32-391">Otherwise, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="9cf32-392">否則，轉換會當做明確參考轉換或身分識別轉換來執行。</span><span class="sxs-lookup"><span data-stu-id="9cf32-392">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="9cf32-393">如果已知 `T` 是參考型別，上述的轉換會分類為明確參考轉換（[明確參考轉換](conversions.md#explicit-reference-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-393">If `T` is known to be a reference type, the conversions above are all classified as explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="9cf32-394">如果 `T` 不知道是參考型別，上述的轉換會分類為「取消裝箱」轉換（[取消裝箱轉換](conversions.md#unboxing-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-394">If `T` is not known to be a reference type, the conversions above are classified as unboxing conversions ([Unboxing conversions](conversions.md#unboxing-conversions)).</span></span>

<span data-ttu-id="9cf32-395">上述規則不允許從不受限制的型別參數直接明確轉換成非介面型別，這可能會令人驚訝。</span><span class="sxs-lookup"><span data-stu-id="9cf32-395">The above rules do not permit a direct explicit conversion from an unconstrained type parameter to a non-interface type, which might be surprising.</span></span> <span data-ttu-id="9cf32-396">此規則的原因是為了避免混淆，並讓這類轉換的語法清楚明瞭。</span><span class="sxs-lookup"><span data-stu-id="9cf32-396">The reason for this rule is to prevent confusion and make the semantics of such conversions clear.</span></span> <span data-ttu-id="9cf32-397">例如，請參考下列宣告：</span><span class="sxs-lookup"><span data-stu-id="9cf32-397">For example, consider the following declaration:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

<span data-ttu-id="9cf32-398">如果允許 `t` 直接明確轉換為 `int`，則可能會輕易地預期 `X<int>.F(7)` 會傳回 `7L`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-398">If the direct explicit conversion of `t` to `int` were permitted, one might easily expect that `X<int>.F(7)` would return `7L`.</span></span> <span data-ttu-id="9cf32-399">不過，它不會這麼做，因為只有在系結時間已知為數值的類型時，才會考慮標準數值轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-399">However, it would not, because the standard numeric conversions are only considered when the types are known to be numeric at binding-time.</span></span> <span data-ttu-id="9cf32-400">為了讓語法清楚明瞭，必須改為撰寫上述範例：</span><span class="sxs-lookup"><span data-stu-id="9cf32-400">In order to make the semantics clear, the above example must instead be written:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

<span data-ttu-id="9cf32-401">這段程式碼現在會進行編譯，但執行 `X<int>.F(7)` 接著會在執行時間擲回例外狀況，因為無法直接將已封裝的 `int` 轉換成 `long`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-401">This code will now compile but executing `X<int>.F(7)` would then throw an exception at run-time, since a boxed `int` cannot be converted directly to a `long`.</span></span>

### <a name="user-defined-explicit-conversions"></a><span data-ttu-id="9cf32-402">使用者定義的明確轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-402">User-defined explicit conversions</span></span>

<span data-ttu-id="9cf32-403">使用者定義的明確轉換是由選擇性的標準明確轉換所組成，接著執行使用者定義的隱含或明確轉換運算子，再接著另一個選擇性的標準明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-403">A user-defined explicit conversion consists of an optional standard explicit conversion, followed by execution of a user-defined implicit or explicit conversion operator, followed by another optional standard explicit conversion.</span></span> <span data-ttu-id="9cf32-404">用於評估使用者定義明確轉換的確切規則，將在[處理使用者定義的明確轉換](conversions.md#processing-of-user-defined-explicit-conversions)中說明。</span><span class="sxs-lookup"><span data-stu-id="9cf32-404">The exact rules for evaluating user-defined explicit conversions are described in [Processing of user-defined explicit conversions](conversions.md#processing-of-user-defined-explicit-conversions).</span></span>

## <a name="standard-conversions"></a><span data-ttu-id="9cf32-405">標準轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-405">Standard conversions</span></span>

<span data-ttu-id="9cf32-406">標準轉換是那些預先定義的轉換，可能會在使用者定義的轉換中發生。</span><span class="sxs-lookup"><span data-stu-id="9cf32-406">The standard conversions are those pre-defined conversions that can occur as part of a user-defined conversion.</span></span>

### <a name="standard-implicit-conversions"></a><span data-ttu-id="9cf32-407">標準隱含轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-407">Standard implicit conversions</span></span>

<span data-ttu-id="9cf32-408">下列隱含轉換會分類為標準隱含轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-408">The following implicit conversions are classified as standard implicit conversions:</span></span>

*  <span data-ttu-id="9cf32-409">身分識別轉換（身分[識別轉換](conversions.md#identity-conversion)）</span><span class="sxs-lookup"><span data-stu-id="9cf32-409">Identity conversions ([Identity conversion](conversions.md#identity-conversion))</span></span>
*  <span data-ttu-id="9cf32-410">隱含數值轉換（[隱含數值轉換](conversions.md#implicit-numeric-conversions)）</span><span class="sxs-lookup"><span data-stu-id="9cf32-410">Implicit numeric conversions ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions))</span></span>
*  <span data-ttu-id="9cf32-411">隱含可為 null 的轉換（[隱含的可為 null 轉換](conversions.md#implicit-nullable-conversions)）</span><span class="sxs-lookup"><span data-stu-id="9cf32-411">Implicit nullable conversions ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions))</span></span>
*  <span data-ttu-id="9cf32-412">隱含參考轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)）</span><span class="sxs-lookup"><span data-stu-id="9cf32-412">Implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
*  <span data-ttu-id="9cf32-413">裝箱轉換（[裝箱轉換](conversions.md#boxing-conversions)）</span><span class="sxs-lookup"><span data-stu-id="9cf32-413">Boxing conversions ([Boxing conversions](conversions.md#boxing-conversions))</span></span>
*  <span data-ttu-id="9cf32-414">隱含常數運算式轉換（[隱含動態轉換](conversions.md#implicit-dynamic-conversions)）</span><span class="sxs-lookup"><span data-stu-id="9cf32-414">Implicit constant expression conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions))</span></span>
*  <span data-ttu-id="9cf32-415">涉及型別參數的隱含轉換（[涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)）</span><span class="sxs-lookup"><span data-stu-id="9cf32-415">Implicit conversions involving type parameters ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters))</span></span>

<span data-ttu-id="9cf32-416">標準隱含轉換會特別排除使用者定義的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-416">The standard implicit conversions specifically exclude user-defined implicit conversions.</span></span>

### <a name="standard-explicit-conversions"></a><span data-ttu-id="9cf32-417">標準明確轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-417">Standard explicit conversions</span></span>

<span data-ttu-id="9cf32-418">標準明確轉換是標準隱含轉換，加上相反標準隱含轉換存在之明確轉換的子集。</span><span class="sxs-lookup"><span data-stu-id="9cf32-418">The standard explicit conversions are all standard implicit conversions plus the subset of the explicit conversions for which an opposite standard implicit conversion exists.</span></span> <span data-ttu-id="9cf32-419">換句話說，如果從型別 `A` 到型別 `B`的標準隱含轉換，則從型別 `A` 到型別 `B` 以及從型別 `B` 到型別 `A`都有標準的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-419">In other words, if a standard implicit conversion exists from a type `A` to a type `B`, then a standard explicit conversion exists from type `A` to type `B` and from type `B` to type `A`.</span></span>

## <a name="user-defined-conversions"></a><span data-ttu-id="9cf32-420">使用者定義轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-420">User-defined conversions</span></span>

<span data-ttu-id="9cf32-421">C#允許透過***使用者定義的轉換***來增強預先定義的隱含和明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-421">C# allows the pre-defined implicit and explicit conversions to be augmented by ***user-defined conversions***.</span></span> <span data-ttu-id="9cf32-422">使用者定義的轉換是藉由在類別和結構類型中宣告轉換運算子（[轉換運算子](classes.md#conversion-operators)）來引入。</span><span class="sxs-lookup"><span data-stu-id="9cf32-422">User-defined conversions are introduced by declaring conversion operators ([Conversion operators](classes.md#conversion-operators)) in class and struct types.</span></span>

### <a name="permitted-user-defined-conversions"></a><span data-ttu-id="9cf32-423">允許的使用者定義轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-423">Permitted user-defined conversions</span></span>

<span data-ttu-id="9cf32-424">C#只允許宣告特定使用者定義的轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-424">C# permits only certain user-defined conversions to be declared.</span></span> <span data-ttu-id="9cf32-425">特別是，您無法重新定義已經存在的隱含或明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-425">In particular, it is not possible to redefine an already existing implicit or explicit conversion.</span></span>

<span data-ttu-id="9cf32-426">對於給定的來源類型 `S` 和目標型別 `T`，如果 `S` 或 `T` 是可為 null 的類型，請讓 `S0` 和 `T0` 參考其基礎類型，否則 `S0` 和 `T0` 會分別等於 `S` 和 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-426">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="9cf32-427">只有在下列所有條件都成立時，才能使用類別或結構來宣告從來源類型 `S` 至目標型別的轉換 `T`：</span><span class="sxs-lookup"><span data-stu-id="9cf32-427">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="9cf32-428">`S0` 和 `T0` 是不同的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-428">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="9cf32-429">`S0` 或 `T0` 是發生運算子宣告的類別或結構類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-429">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="9cf32-430">`S0` 或 `T0` 都不是*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="9cf32-430">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="9cf32-431">除了使用者定義的轉換之外，從 `S` 到 `T` 或從 `T` 到 `S`的轉換不存在。</span><span class="sxs-lookup"><span data-stu-id="9cf32-431">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="9cf32-432">適用于使用者定義轉換的限制會在[轉換運算子](classes.md#conversion-operators)中進一步討論。</span><span class="sxs-lookup"><span data-stu-id="9cf32-432">The restrictions that apply to user-defined conversions are discussed further in [Conversion operators](classes.md#conversion-operators).</span></span>

### <a name="lifted-conversion-operators"></a><span data-ttu-id="9cf32-433">提升的轉換運算子</span><span class="sxs-lookup"><span data-stu-id="9cf32-433">Lifted conversion operators</span></span>

<span data-ttu-id="9cf32-434">假設使用者定義的轉換運算子從不可為 null 的實值型別轉換 `S` 到不可為 null 的實值型別 `T`，則會有一個從 `S?` 轉換成 `T?`的***提升轉換運算子***。</span><span class="sxs-lookup"><span data-stu-id="9cf32-434">Given a user-defined conversion operator that converts from a non-nullable value type `S` to a non-nullable value type `T`, a ***lifted conversion operator*** exists that converts from `S?` to `T?`.</span></span> <span data-ttu-id="9cf32-435">這個提升轉換運算子會從 `S?` 解除包裝到 `S` 後面接著使用者定義的從 `S` 轉換成 `T`，然後再從 `T` 換成 `T?`，但是 null 值 `S?` 會直接轉換成 null 值 `T?`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-435">This lifted conversion operator performs an unwrapping from `S?` to `S` followed by the user-defined conversion from `S` to `T` followed by a wrapping from `T` to `T?`, except that a null valued `S?` converts directly to a null valued `T?`.</span></span>

<span data-ttu-id="9cf32-436">提升轉換運算子與其基礎使用者定義的轉換運算子具有相同的隱含或明確分類。</span><span class="sxs-lookup"><span data-stu-id="9cf32-436">A lifted conversion operator has the same implicit or explicit classification as its underlying user-defined conversion operator.</span></span> <span data-ttu-id="9cf32-437">「使用者定義轉換」一詞適用于使用者定義和提升轉換運算子的使用。</span><span class="sxs-lookup"><span data-stu-id="9cf32-437">The term "user-defined conversion" applies to the use of both user-defined and lifted conversion operators.</span></span>

### <a name="evaluation-of-user-defined-conversions"></a><span data-ttu-id="9cf32-438">評估使用者定義的轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-438">Evaluation of user-defined conversions</span></span>

<span data-ttu-id="9cf32-439">使用者定義的轉換會將其類型的值（稱為「***來源類型***」）轉換成另一種類型，稱為「***目標型別」（target type***）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-439">A user-defined conversion converts a value from its type, called the ***source type***, to another type, called the ***target type***.</span></span> <span data-ttu-id="9cf32-440">評估使用者定義的轉換中心，以尋找特定來源和目標型別的***最特定***使用者定義轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="9cf32-440">Evaluation of a user-defined conversion centers on finding the ***most specific*** user-defined conversion operator for the particular source and target types.</span></span> <span data-ttu-id="9cf32-441">這項判斷分為數個步驟：</span><span class="sxs-lookup"><span data-stu-id="9cf32-441">This determination is broken into several steps:</span></span>

*  <span data-ttu-id="9cf32-442">尋找將視為使用者定義轉換運算子的一組類別和結構。</span><span class="sxs-lookup"><span data-stu-id="9cf32-442">Finding the set of classes and structs from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="9cf32-443">這個集合是由來源類型及其基類和目標型別及其基類所組成（隱含假設只有類別和結構可以宣告使用者定義的運算子，而且該非類別的類型沒有基類）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-443">This set consists of the source type and its base classes and the target type and its base classes (with the implicit assumptions that only classes and structs can declare user-defined operators, and that non-class types have no base classes).</span></span> <span data-ttu-id="9cf32-444">基於此步驟的目的，如果來源或目標型別為*nullable_type*，則會改用其基礎類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-444">For the purposes of this step, if either the source or target type is a *nullable_type*, their underlying type is used instead.</span></span>
*  <span data-ttu-id="9cf32-445">從該類型集合中，判斷適用的使用者定義和提升轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="9cf32-445">From that set of types, determining which user-defined and lifted conversion operators are applicable.</span></span> <span data-ttu-id="9cf32-446">若要讓轉換運算子適用，必須從來源類型執行標準轉換（[標準](conversions.md#standard-conversions)轉換）為運算子的運算元類型，而且必須能夠從運算子的結果型別執行標準轉換至目標型別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-446">For a conversion operator to be applicable, it must be possible to perform a standard conversion ([Standard conversions](conversions.md#standard-conversions)) from the source type to the operand type of the operator, and it must be possible to perform a standard conversion from the result type of the operator to the target type.</span></span>
*  <span data-ttu-id="9cf32-447">從一組適用的使用者定義運算子，判斷哪一個運算子明確是最明確的。</span><span class="sxs-lookup"><span data-stu-id="9cf32-447">From the set of applicable user-defined operators, determining which operator is unambiguously the most specific.</span></span> <span data-ttu-id="9cf32-448">一般來說，最特定的運算子是運算子，其運算元類型會「最接近」來源類型，且其結果類型會「最接近」目標型別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-448">In general terms, the most specific operator is the operator whose operand type is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="9cf32-449">使用者定義的轉換運算子優先于提升的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="9cf32-449">User-defined conversion operators are preferred over lifted conversion operators.</span></span> <span data-ttu-id="9cf32-450">建立最特定使用者定義轉換運算子的確切規則定義于下列各節中。</span><span class="sxs-lookup"><span data-stu-id="9cf32-450">The exact rules for establishing the most specific user-defined conversion operator are defined in the following sections.</span></span>

<span data-ttu-id="9cf32-451">一旦識別出最特定的使用者定義轉換運算子之後，實際執行的使用者定義轉換就會牽涉到三個步驟：</span><span class="sxs-lookup"><span data-stu-id="9cf32-451">Once a most specific user-defined conversion operator has been identified, the actual execution of the user-defined conversion involves up to three steps:</span></span>

*  <span data-ttu-id="9cf32-452">首先，如有必要，執行從來源類型到使用者定義或提升轉換運算子之運算元類型的標準轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-452">First, if required, performing a standard conversion from the source type to the operand type of the user-defined or lifted conversion operator.</span></span>
*  <span data-ttu-id="9cf32-453">接下來，叫用使用者定義或提升的轉換運算子，以執行轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-453">Next, invoking the user-defined or lifted conversion operator to perform the conversion.</span></span>
*  <span data-ttu-id="9cf32-454">最後，如有必要，從使用者定義或提升轉換運算子的結果型別執行標準轉換至目標型別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-454">Finally, if required, performing a standard conversion from the result type of the user-defined or lifted conversion operator to the target type.</span></span>

<span data-ttu-id="9cf32-455">評估使用者定義的轉換絕不會牽涉到一個以上的使用者定義或提升轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="9cf32-455">Evaluation of a user-defined conversion never involves more than one user-defined or lifted conversion operator.</span></span> <span data-ttu-id="9cf32-456">換句話說，從類型 `S` 到類型 `T` 的轉換永遠不會先從 `X` `S` 執行使用者定義的轉換，然後再從 `X` 執行使用者定義的轉換至 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-456">In other words, a conversion from type `S` to type `T` will never first execute a user-defined conversion from `S` to `X` and then execute a user-defined conversion from `X` to `T`.</span></span>

<span data-ttu-id="9cf32-457">下列各節提供使用者定義隱含或明確轉換的確切評估定義。</span><span class="sxs-lookup"><span data-stu-id="9cf32-457">Exact definitions of evaluation of user-defined implicit or explicit conversions are given in the following sections.</span></span> <span data-ttu-id="9cf32-458">定義會使用下列詞彙：</span><span class="sxs-lookup"><span data-stu-id="9cf32-458">The definitions make use of the following terms:</span></span>

*  <span data-ttu-id="9cf32-459">如果標準隱含轉換（[標準隱含](conversions.md#standard-implicit-conversions)轉換）存在於類型 `A` `B`，且 `A` 或 `B` 都不是*interface_type*s，則 `A` 會被視為***包含***`B`，而 `B` 則稱為***包含***`A`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-459">If a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) exists from a type `A` to a type `B`, and if neither `A` nor `B` are *interface_type*s, then `A` is said to be ***encompassed by*** `B`, and `B` is said to ***encompass*** `A`.</span></span>
*  <span data-ttu-id="9cf32-460">一組類型中最包含的***類型***，是一種包含集合中所有其他類型的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-460">The ***most encompassing type*** in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="9cf32-461">如果沒有單一類型包含所有其他類型，則該集合沒有最多包含的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-461">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="9cf32-462">更直覺的說，最包含的型別是集合中的「最大」型別，也就是每個其他型別都可以隱含轉換成的一種類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-462">In more intuitive terms, the most encompassing type is the "largest" type in the set—the one type to which each of the other types can be implicitly converted.</span></span>
*  <span data-ttu-id="9cf32-463">一組類型中***最包含的類型***，就是集合中所有其他類型所包含的一種類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-463">The ***most encompassed type*** in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="9cf32-464">如果沒有任何其他類型包含任何單一類型，則該集合不會包含大部分包含的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-464">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="9cf32-465">更直覺的說，最包含的型別是集合中的「最小」型別，也就是可以隱含地轉換成每個其他型別的型別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-465">In more intuitive terms, the most encompassed type is the "smallest" type in the set—the one type that can be implicitly converted to each of the other types.</span></span>

### <a name="processing-of-user-defined-implicit-conversions"></a><span data-ttu-id="9cf32-466">使用者定義隱含轉換的處理</span><span class="sxs-lookup"><span data-stu-id="9cf32-466">Processing of user-defined implicit conversions</span></span>

<span data-ttu-id="9cf32-467">從類型 `S` 到類型 `T` 的使用者定義隱含轉換，會依照下列方式處理：</span><span class="sxs-lookup"><span data-stu-id="9cf32-467">A user-defined implicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="9cf32-468">判斷 `S0` 和 `T0`的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-468">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="9cf32-469">如果 `S` 或 `T` 是可為 null 的類型，`S0` 和 `T0` 是其基礎類型，否則 `S0` 和 `T0` 分別等於 `S` 和 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-469">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="9cf32-470">尋找要將使用者定義的轉換運算子視為其來源的類型集合（`D`）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-470">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="9cf32-471">這個集合包含 `S0` （如果 `S0` 是類別或結構）、`S0` 的基類（如果 `S0` 是類別），以及 `T0` （如果 `T0` 是類別或結構）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-471">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), and `T0` (if `T0` is a class or struct).</span></span>
*  <span data-ttu-id="9cf32-472">尋找一組適用的使用者定義和提升轉換運算子，`U`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-472">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="9cf32-473">這個集合是由 `D` 中的類別或結構所宣告的使用者定義和提升隱含轉換運算子所組成，而這些運算式會從包含 `S` 的類型轉換成由 `T`所組成的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-473">This set consists of the user-defined and lifted implicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing `S` to a type encompassed by `T`.</span></span> <span data-ttu-id="9cf32-474">如果 `U` 是空的，則轉換會是未定義的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-474">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="9cf32-475">尋找 `U`中運算子的最特定來源類型 `SX`：</span><span class="sxs-lookup"><span data-stu-id="9cf32-475">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="9cf32-476">如果 `U` 中的任何運算子從 `S`轉換，則 `SX` 會 `S`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-476">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="9cf32-477">否則，`SX` 是 `U`中運算子的結合一組來源類型的最包含類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-477">Otherwise, `SX` is the most encompassed type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="9cf32-478">如果找不到其中一個最包含的類型，則轉換會是不明確的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-478">If exactly one most encompassed type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="9cf32-479">找出 `U`中運算子的最特定目標型別 `TX`：</span><span class="sxs-lookup"><span data-stu-id="9cf32-479">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="9cf32-480">如果 `U` 中的任何運算子轉換成 `T`，則 `TX` 會 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-480">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="9cf32-481">否則，`TX` 是 `U`之運算子的一組目標型別中最包含的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-481">Otherwise, `TX` is the most encompassing type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="9cf32-482">如果找不到一個最包含的類型，則轉換會是不明確的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-482">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="9cf32-483">尋找最特定的轉換運算子：</span><span class="sxs-lookup"><span data-stu-id="9cf32-483">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="9cf32-484">如果 `U` 只包含一個從 `SX` 轉換為 `TX`的使用者定義轉換運算子，則這是最特定的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="9cf32-484">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="9cf32-485">否則，如果 `U` 只包含一個從 `SX` 轉換為 `TX`的提升轉換運算子，則這是最特定的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="9cf32-485">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="9cf32-486">否則，轉換會是不明確的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-486">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="9cf32-487">最後，套用轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-487">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="9cf32-488">如果未 `SX``S`，則會執行從 `S` 到 `SX` 的標準隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-488">If `S` is not `SX`, then a standard implicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="9cf32-489">叫用最特定的轉換運算子，以從 `SX` 轉換成 `TX`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-489">The most specific conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="9cf32-490">如果未 `T``TX`，則會執行從 `TX` 到 `T` 的標準隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-490">If `TX` is not `T`, then a standard implicit conversion from `TX` to `T` is performed.</span></span>

### <a name="processing-of-user-defined-explicit-conversions"></a><span data-ttu-id="9cf32-491">使用者定義的明確轉換的處理</span><span class="sxs-lookup"><span data-stu-id="9cf32-491">Processing of user-defined explicit conversions</span></span>

<span data-ttu-id="9cf32-492">從類型 `S` 到類型 `T` 的使用者定義明確轉換會依照下列方式處理：</span><span class="sxs-lookup"><span data-stu-id="9cf32-492">A user-defined explicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="9cf32-493">判斷 `S0` 和 `T0`的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-493">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="9cf32-494">如果 `S` 或 `T` 是可為 null 的類型，`S0` 和 `T0` 是其基礎類型，否則 `S0` 和 `T0` 分別等於 `S` 和 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-494">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="9cf32-495">尋找要將使用者定義的轉換運算子視為其來源的類型集合（`D`）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-495">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="9cf32-496">這個集合包含 `S0` （如果 `S0` 是類別或結構）、`S0` 的基類（如果 `S0` 是類別）、`T0` （如果 `T0` 是類別或結構），以及 `T0` 的基類（如果 `T0` 是類別）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-496">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), `T0` (if `T0` is a class or struct), and the base classes of `T0` (if `T0` is a class).</span></span>
*  <span data-ttu-id="9cf32-497">尋找一組適用的使用者定義和提升轉換運算子，`U`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-497">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="9cf32-498">這個集合是由 `D` 中的類別或結構所宣告的使用者定義和提升的隱含或明確轉換運算子所組成，而這些方法會從包含或包含在 `S` 的類型轉換成包含或包含 `T`的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-498">This set consists of the user-defined and lifted implicit or explicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing or encompassed by `S` to a type encompassing or encompassed by `T`.</span></span> <span data-ttu-id="9cf32-499">如果 `U` 是空的，則轉換會是未定義的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-499">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="9cf32-500">尋找 `U`中運算子的最特定來源類型 `SX`：</span><span class="sxs-lookup"><span data-stu-id="9cf32-500">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="9cf32-501">如果 `U` 中的任何運算子從 `S`轉換，則 `SX` 會 `S`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-501">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="9cf32-502">否則，如果 `U` 中的任何運算子從包含 `S`的類型進行轉換，則 `SX` 是這些運算子的一組來源類型中最包含的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-502">Otherwise, if any of the operators in `U` convert from types that encompass `S`, then `SX` is the most encompassed type in the combined set of source types of those operators.</span></span> <span data-ttu-id="9cf32-503">如果找不到最包含的類型，則轉換會不明確，且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-503">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="9cf32-504">否則，`SX` 是 `U`中運算子的結合一組來源類型的最包含類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-504">Otherwise, `SX` is the most encompassing type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="9cf32-505">如果找不到一個最包含的類型，則轉換會是不明確的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-505">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="9cf32-506">找出 `U`中運算子的最特定目標型別 `TX`：</span><span class="sxs-lookup"><span data-stu-id="9cf32-506">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="9cf32-507">如果 `U` 中的任何運算子轉換成 `T`，則 `TX` 會 `T`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-507">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="9cf32-508">否則，如果 `U` 中的任何運算子轉換成 `T`所包含的類型，則 `TX` 是這些運算子的一組目標型別中最包含的類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-508">Otherwise, if any of the operators in `U` convert to types that are encompassed by `T`, then `TX` is the most encompassing type in the combined set of target types of those operators.</span></span> <span data-ttu-id="9cf32-509">如果找不到一個最包含的類型，則轉換會是不明確的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-509">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="9cf32-510">否則，`TX` 是 `U`中運算子的一組目標型別的最包含類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-510">Otherwise, `TX` is the most encompassed type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="9cf32-511">如果找不到最包含的類型，則轉換會不明確，且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-511">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="9cf32-512">尋找最特定的轉換運算子：</span><span class="sxs-lookup"><span data-stu-id="9cf32-512">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="9cf32-513">如果 `U` 只包含一個從 `SX` 轉換為 `TX`的使用者定義轉換運算子，則這是最特定的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="9cf32-513">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="9cf32-514">否則，如果 `U` 只包含一個從 `SX` 轉換為 `TX`的提升轉換運算子，則這是最特定的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="9cf32-514">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="9cf32-515">否則，轉換會是不明確的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-515">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="9cf32-516">最後，套用轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-516">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="9cf32-517">如果未 `SX``S`，則會執行從 `S` 到 `SX` 的標準明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-517">If `S` is not `SX`, then a standard explicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="9cf32-518">叫用最特定的使用者定義轉換運算子，以從 `SX` 轉換成 `TX`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-518">The most specific user-defined conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="9cf32-519">如果未 `T``TX`，則會執行從 `TX` 到 `T` 的標準明確轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-519">If `TX` is not `T`, then a standard explicit conversion from `TX` to `T` is performed.</span></span>

## <a name="anonymous-function-conversions"></a><span data-ttu-id="9cf32-520">匿名函數轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-520">Anonymous function conversions</span></span>

<span data-ttu-id="9cf32-521">*Anonymous_method_expression*或*lambda_expression*會分類為匿名函式（[匿名函數運算式](expressions.md#anonymous-function-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-521">An *anonymous_method_expression* or *lambda_expression* is classified as an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="9cf32-522">運算式沒有類型，但可以隱含地轉換成相容的委派類型或運算式樹狀架構類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-522">The expression does not have a type but can be implicitly converted to a compatible delegate type or expression tree type.</span></span> <span data-ttu-id="9cf32-523">具體而言，匿名函數 `F` 與 `D` 提供的委派類型相容：</span><span class="sxs-lookup"><span data-stu-id="9cf32-523">Specifically, an anonymous function `F` is compatible with a delegate type `D` provided:</span></span>

*  <span data-ttu-id="9cf32-524">如果 `F` 包含*anonymous_function_signature*，則 `D` 和 `F` 的參數數目相同。</span><span class="sxs-lookup"><span data-stu-id="9cf32-524">If `F` contains an *anonymous_function_signature*, then `D` and `F` have the same number of parameters.</span></span>
*  <span data-ttu-id="9cf32-525">如果 `F` 不包含*anonymous_function_signature*，則 `D` 可能會有任何類型的零或多個參數，只要 `D` 的參數沒有 `out` 參數修飾詞。</span><span class="sxs-lookup"><span data-stu-id="9cf32-525">If `F` does not contain an *anonymous_function_signature*, then `D` may have zero or more parameters of any type, as long as no parameter of `D` has the `out` parameter modifier.</span></span>
*  <span data-ttu-id="9cf32-526">如果 `F` 有明確類型的參數清單，則 `D` 中的每個參數都與 `F`中的對應參數具有相同的類型和修飾詞。</span><span class="sxs-lookup"><span data-stu-id="9cf32-526">If `F` has an explicitly typed parameter list, each parameter in `D` has the same type and modifiers as the corresponding parameter in `F`.</span></span>
*  <span data-ttu-id="9cf32-527">如果 `F` 有隱含類型的參數清單，`D` 就不會有 `ref` 或 `out` 參數。</span><span class="sxs-lookup"><span data-stu-id="9cf32-527">If `F` has an implicitly typed parameter list, `D` has no `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="9cf32-528">如果 `F` 的主體是運算式，而且 `D` 有 `void` 傳回型別或 `F` 是非同步，而且 `D` 的傳回型別 `Task`，則當 `F` 的每個參數都指定 `D`中對應參數的型別時，`F` 的主體就是一個有效的運算式（wrt[運算式](expressions.md)），允許做為*statement_expression* （[expression 語句](statements.md#expression-statements)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-528">If the body of `F` is an expression, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that would be permitted as a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>
*  <span data-ttu-id="9cf32-529">如果 `F` 的主體是語句區塊，而且 `D` 有 `void` 傳回類型或 `F` 是非同步，而且 `D` 具有傳回類型 `Task`，則當 `F` 的每個參數都指定 `D`中對應參數的類型時，`F` 的主體就是有效的語句區塊[（wrt 組塊](statements.md#blocks)），其中沒有任何 `return` 語句指定運算式。</span><span class="sxs-lookup"><span data-stu-id="9cf32-529">If the body of `F` is a statement block, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) in which no `return` statement specifies an expression.</span></span>
*  <span data-ttu-id="9cf32-530">如果 `F` 的主體是運算式，*而且 `F` 是*非非同步，而且 `D` 具有非 void 的傳回類型 `T`，*或*`F` 是非同步，而且 `D` 的傳回類型 `Task<T>`，則 `F` 的主體會是有效的運算式（wrt[運算式](expressions.md)），可以隱含地轉換成 `D`中的對應參數。</span><span class="sxs-lookup"><span data-stu-id="9cf32-530">If the body of `F` is an expression, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that is implicitly convertible to `T`.</span></span>
*  <span data-ttu-id="9cf32-531">如果 `F` 的主體為語句區塊，且 `F` 為非非同步 *，而且 `D`* 具有非 void 的傳回類型 `T`，*或*`F` 是非同步，`D` 有 `Task<T>`的傳回型別，則當 `F` 的每個參數都指定 `D`中對應參數的型別時，`F` 的主體就是有效的語句區塊[（wrt 組塊](statements.md#blocks)），其中每個 `return` 語句都會指定一個可隱含轉換成 `T`的運算式。</span><span class="sxs-lookup"><span data-stu-id="9cf32-531">If the body of `F` is a statement block, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) with a non-reachable end point in which each `return` statement specifies an expression that is implicitly convertible to `T`.</span></span>

<span data-ttu-id="9cf32-532">為了簡潔起見，本節使用工作類型的簡短形式 `Task` 和 `Task<T>` （[非同步](classes.md#async-functions)函式）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-532">For the purpose of brevity, this section uses the short form for the task types `Task` and `Task<T>` ([Async functions](classes.md#async-functions)).</span></span>

<span data-ttu-id="9cf32-533">如果 `F` 與 `D`的委派類型相容，則 lambda 運算式 `F` 與運算式樹狀架構類型相容 `Expression<D>`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-533">A lambda expression `F` is compatible with an expression tree type `Expression<D>` if `F` is compatible with the delegate type `D`.</span></span> <span data-ttu-id="9cf32-534">請注意，這不會套用到匿名方法，只適用于 lambda 運算式。</span><span class="sxs-lookup"><span data-stu-id="9cf32-534">Note that this does not apply to anonymous methods, only lambda expressions.</span></span>

<span data-ttu-id="9cf32-535">某些 lambda 運算式無法轉換成運算式樹狀架構類型：雖然轉換*存在*，但它會在編譯時期失敗。</span><span class="sxs-lookup"><span data-stu-id="9cf32-535">Certain lambda expressions cannot be converted to expression tree types: Even though the conversion *exists*, it fails at compile-time.</span></span> <span data-ttu-id="9cf32-536">如果是 lambda 運算式，就會發生這種情況：</span><span class="sxs-lookup"><span data-stu-id="9cf32-536">This is the case if the lambda expression:</span></span>

*  <span data-ttu-id="9cf32-537">具有*區塊*主體</span><span class="sxs-lookup"><span data-stu-id="9cf32-537">Has a *block* body</span></span>
*  <span data-ttu-id="9cf32-538">包含簡單或複合指派運算子</span><span class="sxs-lookup"><span data-stu-id="9cf32-538">Contains simple or compound assignment operators</span></span>
*  <span data-ttu-id="9cf32-539">包含動態繫結運算式</span><span class="sxs-lookup"><span data-stu-id="9cf32-539">Contains a dynamically bound expression</span></span>
*  <span data-ttu-id="9cf32-540">為 async</span><span class="sxs-lookup"><span data-stu-id="9cf32-540">Is async</span></span>

<span data-ttu-id="9cf32-541">接下來的範例會使用泛型委派型別，`Func<A,R>` 代表接受 `A` 型別之引數的函式，並傳回 `R`型別的值：</span><span class="sxs-lookup"><span data-stu-id="9cf32-541">The examples that follow use a generic delegate type `Func<A,R>` which represents a function that takes an argument of type `A` and returns a value of type `R`:</span></span>
```csharp
delegate R Func<A,R>(A arg);
```

<span data-ttu-id="9cf32-542">在指派中</span><span class="sxs-lookup"><span data-stu-id="9cf32-542">In the assignments</span></span>
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
<span data-ttu-id="9cf32-543">每個匿名函式的參數和傳回型別，都是根據指派匿名函式的變數類型來決定。</span><span class="sxs-lookup"><span data-stu-id="9cf32-543">the parameter and return types of each anonymous function are determined from the type of the variable to which the anonymous function is assigned.</span></span>

<span data-ttu-id="9cf32-544">第一個指派會成功將匿名函式轉換為委派類型 `Func<int,int>` 因為，當 `x` 是指定的類型 `int`時，`x+1` 是可隱含轉換成類型 `int`的有效運算式。</span><span class="sxs-lookup"><span data-stu-id="9cf32-544">The first assignment successfully converts the anonymous function to the delegate type `Func<int,int>` because, when `x` is given type `int`, `x+1` is a valid expression that is implicitly convertible to type `int`.</span></span>

<span data-ttu-id="9cf32-545">同樣地，第二個指派會成功將匿名函式轉換為委派類型 `Func<int,double>` 因為 `x+1` （類型 `int`）的結果會隱含地轉換成類型 `double`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-545">Likewise, the second assignment successfully converts the anonymous function to the delegate type `Func<int,double>` because the result of `x+1` (of type `int`) is implicitly convertible to type `double`.</span></span>

<span data-ttu-id="9cf32-546">不過，第三個指派是編譯時期錯誤，因為當 `x` 的類型 `double`時，`x+1` （類型 `double`）的結果不會隱含地轉換成類型 `int`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-546">However, the third assignment is a compile-time error because, when `x` is given type `double`, the result of `x+1` (of type `double`) is not implicitly convertible to type `int`.</span></span>

<span data-ttu-id="9cf32-547">第四個指派會成功將匿名非同步函式轉換為委派類型 `Func<int, Task<int>>` 因為 `x+1` （類型 `int`）的結果會隱含地轉換成工作類型 `Task<int>`的結果類型 `int`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-547">The fourth assignment successfully converts the anonymous async function to the delegate type `Func<int, Task<int>>` because the result of `x+1` (of type `int`) is implicitly convertible to the result type `int` of the task type `Task<int>`.</span></span>

<span data-ttu-id="9cf32-548">匿名函數可能會影響多載解析，並參與型別推斷。</span><span class="sxs-lookup"><span data-stu-id="9cf32-548">Anonymous functions may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="9cf32-549">如需進一步的詳細資料，請參閱[函數成員](expressions.md#function-members)。</span><span class="sxs-lookup"><span data-stu-id="9cf32-549">See [Function members](expressions.md#function-members) for further details.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a><span data-ttu-id="9cf32-550">評估委派類型的匿名函式轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-550">Evaluation of anonymous function conversions to delegate types</span></span>

<span data-ttu-id="9cf32-551">將匿名函式轉換成委派類型時，會產生參考匿名函式的委派實例，以及在評估時作用中的已捕捉外部變數（可能是空的）集。</span><span class="sxs-lookup"><span data-stu-id="9cf32-551">Conversion of an anonymous function to a delegate type produces a delegate instance which references the anonymous function and the (possibly empty) set of captured outer variables that are active at the time of the evaluation.</span></span> <span data-ttu-id="9cf32-552">叫用委派時，會執行匿名函式的主體。</span><span class="sxs-lookup"><span data-stu-id="9cf32-552">When the delegate is invoked, the body of the anonymous function is executed.</span></span> <span data-ttu-id="9cf32-553">主體中的程式碼會使用委派所參考的一組捕捉外部變數來執行。</span><span class="sxs-lookup"><span data-stu-id="9cf32-553">The code in the body is executed using the set of captured outer variables referenced by the delegate.</span></span>

<span data-ttu-id="9cf32-554">從匿名函式產生之委派的調用清單包含單一專案。</span><span class="sxs-lookup"><span data-stu-id="9cf32-554">The invocation list of a delegate produced from an anonymous function contains a single entry.</span></span> <span data-ttu-id="9cf32-555">未指定委派的確切目標物件和目標方法。</span><span class="sxs-lookup"><span data-stu-id="9cf32-555">The exact target object and target method of the delegate are unspecified.</span></span> <span data-ttu-id="9cf32-556">特別是，它不會指定委派的目標物件是 `null`、封入函式成員的 `this` 值，或其他物件。</span><span class="sxs-lookup"><span data-stu-id="9cf32-556">In particular, it is unspecified whether the target object of the delegate is `null`, the `this` value of the enclosing function member, or some other object.</span></span>

<span data-ttu-id="9cf32-557">允許（但非必要）將具有相同（可能是空的）集的匿名函數轉換成相同的委派類型，以傳回相同的委派實例。</span><span class="sxs-lookup"><span data-stu-id="9cf32-557">Conversions of semantically identical anonymous functions with the same (possibly empty) set of captured outer variable instances to the same delegate types are permitted (but not required) to return the same delegate instance.</span></span> <span data-ttu-id="9cf32-558">這裡使用「語義相同」一詞，這表示在所有情況下，匿名函式的執行會產生相同的效果，並提供相同的引數。</span><span class="sxs-lookup"><span data-stu-id="9cf32-558">The term semantically identical is used here to mean that execution of the anonymous functions will, in all cases, produce the same effects given the same arguments.</span></span> <span data-ttu-id="9cf32-559">此規則允許將下列程式碼優化。</span><span class="sxs-lookup"><span data-stu-id="9cf32-559">This rule permits code such as the following to be optimized.</span></span>

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

<span data-ttu-id="9cf32-560">由於這兩個匿名函式委派具有相同（空白）的已捕捉外部變數集，而且因為匿名函式在語義上相同，所以允許編譯器讓委派參考相同的目標方法。</span><span class="sxs-lookup"><span data-stu-id="9cf32-560">Since the two anonymous function delegates have the same (empty) set of captured outer variables, and since the anonymous functions are semantically identical, the compiler is permitted to have the delegates refer to the same target method.</span></span> <span data-ttu-id="9cf32-561">事實上，編譯器允許從這兩個匿名函式運算式傳回非常相同的委派實例。</span><span class="sxs-lookup"><span data-stu-id="9cf32-561">Indeed, the compiler is permitted to return the very same delegate instance from both anonymous function expressions.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a><span data-ttu-id="9cf32-562">評估匿名函數轉換為運算式樹狀架構類型</span><span class="sxs-lookup"><span data-stu-id="9cf32-562">Evaluation of anonymous function conversions to expression tree types</span></span>

<span data-ttu-id="9cf32-563">將匿名函式轉換成運算式樹狀架構類型會產生運算式樹狀架構（[運算式樹狀架構類型](types.md#expression-tree-types)）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-563">Conversion of an anonymous function to an expression tree type produces an expression tree ([Expression tree types](types.md#expression-tree-types)).</span></span> <span data-ttu-id="9cf32-564">更精確地說，匿名函式轉換的評估會導致物件結構的結構表示匿名函數本身的結構。</span><span class="sxs-lookup"><span data-stu-id="9cf32-564">More precisely, evaluation of the anonymous function conversion leads to the construction of an object structure that represents the structure of the anonymous function itself.</span></span> <span data-ttu-id="9cf32-565">運算式樹狀架構的精確結構，以及建立它的確切程式，都是定義的執行。</span><span class="sxs-lookup"><span data-stu-id="9cf32-565">The precise structure of the expression tree, as well as the exact process for creating it, are implementation defined.</span></span>

### <a name="implementation-example"></a><span data-ttu-id="9cf32-566">實作範例</span><span class="sxs-lookup"><span data-stu-id="9cf32-566">Implementation example</span></span>

<span data-ttu-id="9cf32-567">本節描述以其他C#結構為依據的匿名函式轉換的可能執行。</span><span class="sxs-lookup"><span data-stu-id="9cf32-567">This section describes a possible implementation of anonymous function conversions in terms of other C# constructs.</span></span> <span data-ttu-id="9cf32-568">這裡所述的執行是以 Microsoft C#編譯器所使用的相同原則為基礎，但它並不是強制執行的，也不是唯一可行的方法。</span><span class="sxs-lookup"><span data-stu-id="9cf32-568">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation, nor is it the only one possible.</span></span> <span data-ttu-id="9cf32-569">它只會簡短提及運算式樹狀架構的轉換，因為它們的確切語義超出此規格的範圍。</span><span class="sxs-lookup"><span data-stu-id="9cf32-569">It only briefly mentions conversions to expression trees, as their exact semantics are outside the scope of this specification.</span></span>

<span data-ttu-id="9cf32-570">本節的其餘部分會提供幾個程式碼範例，其中包含具有不同特性的匿名函式。</span><span class="sxs-lookup"><span data-stu-id="9cf32-570">The remainder of this section gives several examples of code that contains anonymous functions with different characteristics.</span></span> <span data-ttu-id="9cf32-571">針對每個範例，會提供只使用其他C#結構之程式碼的對應轉譯。</span><span class="sxs-lookup"><span data-stu-id="9cf32-571">For each example, a corresponding translation to code that uses only other C# constructs is provided.</span></span> <span data-ttu-id="9cf32-572">在範例中，會假設識別碼 `D` 代表下列委派類型：</span><span class="sxs-lookup"><span data-stu-id="9cf32-572">In the examples, the identifier `D` is assumed by represent the following delegate type:</span></span>
```csharp
public delegate void D();
```

<span data-ttu-id="9cf32-573">匿名函式的最簡單形式是不會捕捉到外部變數的函數：</span><span class="sxs-lookup"><span data-stu-id="9cf32-573">The simplest form of an anonymous function is one that captures no outer variables:</span></span>
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

<span data-ttu-id="9cf32-574">這可以轉譯成委派具現化，該實例會參考編譯器產生的靜態方法，其中會放置匿名函式的程式碼：</span><span class="sxs-lookup"><span data-stu-id="9cf32-574">This can be translated to a delegate instantiation that references a compiler generated static method in which the code of the anonymous function is placed:</span></span>
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

<span data-ttu-id="9cf32-575">在下列範例中，匿名函數會參考 `this`的實例成員：</span><span class="sxs-lookup"><span data-stu-id="9cf32-575">In the following example, the anonymous function references instance members of `this`:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

<span data-ttu-id="9cf32-576">這可以轉譯為編譯器產生的實例方法，其中包含匿名函式的程式碼：</span><span class="sxs-lookup"><span data-stu-id="9cf32-576">This can be translated to a compiler generated instance method containing the code of the anonymous function:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

<span data-ttu-id="9cf32-577">在此範例中，匿名函式會捕捉本機變數：</span><span class="sxs-lookup"><span data-stu-id="9cf32-577">In this example, the anonymous function captures a local variable:</span></span>
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

<span data-ttu-id="9cf32-578">本機變數的存留期現在必須至少延伸至匿名函式委派的存留期。</span><span class="sxs-lookup"><span data-stu-id="9cf32-578">The lifetime of the local variable must now be extended to at least the lifetime of the anonymous function delegate.</span></span> <span data-ttu-id="9cf32-579">這可以藉由將本機變數「hoisting」到編譯器所產生類別的欄位來達成。</span><span class="sxs-lookup"><span data-stu-id="9cf32-579">This can be achieved by "hoisting" the local variable into a field of a compiler generated class.</span></span> <span data-ttu-id="9cf32-580">區域變數的具現化（[區域變數的](expressions.md#instantiation-of-local-variables)具現化）接著會對應到建立編譯器所產生類別的實例，而存取區域變數會對應到在編譯器產生之類別的實例中存取欄位。</span><span class="sxs-lookup"><span data-stu-id="9cf32-580">Instantiation of the local variable ([Instantiation of local variables](expressions.md#instantiation-of-local-variables)) then corresponds to creating an instance of the compiler generated class, and accessing the local variable corresponds to accessing a field in the instance of the compiler generated class.</span></span> <span data-ttu-id="9cf32-581">此外，匿名函式會成為編譯器所產生類別的實例方法：</span><span class="sxs-lookup"><span data-stu-id="9cf32-581">Furthermore, the anonymous function becomes an instance method of the compiler generated class:</span></span>
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

<span data-ttu-id="9cf32-582">最後，下列匿名函式會捕捉 `this`，以及兩個具有不同存留期的本機變數：</span><span class="sxs-lookup"><span data-stu-id="9cf32-582">Finally, the following anonymous function captures `this` as well as two local variables with different lifetimes:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

<span data-ttu-id="9cf32-583">在這裡，會針對每個語句區塊建立編譯器產生的類別，其中會針對其中的區域變數進行攔截，讓不同區塊中的區域變數可以有獨立的存留期。</span><span class="sxs-lookup"><span data-stu-id="9cf32-583">Here, a compiler generated class is created for each statement block in which locals are captured such that the locals in the different blocks can have independent lifetimes.</span></span> <span data-ttu-id="9cf32-584">`__Locals2`的實例（內部語句區塊的編譯器產生的類別）包含區域變數 `z` 和參考 `__Locals1`實例的欄位。</span><span class="sxs-lookup"><span data-stu-id="9cf32-584">An instance of `__Locals2`, the compiler generated class for the inner statement block, contains the local variable `z` and a field that references an instance of `__Locals1`.</span></span>  <span data-ttu-id="9cf32-585">`__Locals1`的實例，這是外部語句區塊的編譯器產生的類別，其中包含區域變數 `y` 和參考封入函式成員 `this` 的欄位。</span><span class="sxs-lookup"><span data-stu-id="9cf32-585">An instance of `__Locals1`, the compiler generated class for the outer statement block, contains the local variable `y` and a field that references `this` of the enclosing function member.</span></span> <span data-ttu-id="9cf32-586">使用這些資料結構，可以透過 `__Local2`的實例來觸及所有已捕捉的外部變數，而匿名函式的程式碼則可實作為該類別的實例方法。</span><span class="sxs-lookup"><span data-stu-id="9cf32-586">With these data structures it is possible to reach all captured outer variables through an instance of `__Local2`, and the code of the anonymous function can thus be implemented as an instance method of that class.</span></span>

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

<span data-ttu-id="9cf32-587">將匿名函式轉換為運算式樹狀架構時，也可以使用此處所套用的相同技術來捕捉區域變數：編譯器所產生之物件的參考可以儲存在運算式樹狀架構中，而本機變數的存取可以是表示為這些物件上的欄位存取。</span><span class="sxs-lookup"><span data-stu-id="9cf32-587">The same technique applied here to capture local variables can also be used when converting anonymous functions to expression trees: References to the compiler generated objects can be stored in the expression tree, and access to the local variables can be represented as field accesses on these objects.</span></span> <span data-ttu-id="9cf32-588">這種方法的優點是，它允許在委派和運算式樹狀結構之間共用「提升」的區域變數。</span><span class="sxs-lookup"><span data-stu-id="9cf32-588">The advantage of this approach is that it allows the "lifted" local variables to be shared between delegates and expression trees.</span></span>

## <a name="method-group-conversions"></a><span data-ttu-id="9cf32-589">方法群組轉換</span><span class="sxs-lookup"><span data-stu-id="9cf32-589">Method group conversions</span></span>

<span data-ttu-id="9cf32-590">隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）會從方法群組（[運算式分類](expressions.md#expression-classifications)）存在至相容的委派類型。</span><span class="sxs-lookup"><span data-stu-id="9cf32-590">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from a method group ([Expression classifications](expressions.md#expression-classifications)) to a compatible delegate type.</span></span> <span data-ttu-id="9cf32-591">假設委派類型 `D` 和分類為方法群組的運算式 `E`，如果 `E` 包含至少一個適用于其正常形式（適用于函式[成員](expressions.md#applicable-function-member)）的方法，並使用 `D`的參數類型和修飾詞所建立的引數清單（如下列所述），則隱含轉換會從 `E` 到 `D`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-591">Given a delegate type `D` and an expression `E` that is classified as a method group, an implicit conversion exists from `E` to `D` if `E` contains at least one method that is applicable in its normal form ([Applicable function member](expressions.md#applicable-function-member)) to an argument list constructed by use of the parameter types and modifiers of `D`, as described in the following.</span></span>

<span data-ttu-id="9cf32-592">從方法群組 `E` 至委派類型 `D` 之轉換的編譯時期應用程式如下所述。</span><span class="sxs-lookup"><span data-stu-id="9cf32-592">The compile-time application of a conversion from a method group `E` to a delegate type `D` is described in the following.</span></span> <span data-ttu-id="9cf32-593">請注意，從 `E` 到 `D` 的隱含轉換並不會保證轉換的編譯時期應用程式將會成功，而不會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-593">Note that the existence of an implicit conversion from `E` to `D` does not guarantee that the compile-time application of the conversion will succeed without error.</span></span>

*  <span data-ttu-id="9cf32-594">選取單一方法 `M`，其對應至表單 `E(A)`的方法調用（[方法調用](expressions.md#method-invocations)），但有下列修改：</span><span class="sxs-lookup"><span data-stu-id="9cf32-594">A single method `M` is selected corresponding to a method invocation ([Method invocations](expressions.md#method-invocations)) of the form `E(A)`, with the following modifications:</span></span>
    * <span data-ttu-id="9cf32-595">引數清單 `A` 是運算式的清單，每一個都會分類為一個變數，並在 `D`的*formal_parameter_list*中，使用對應參數的類型和修飾詞（`ref` 或 `out`）。</span><span class="sxs-lookup"><span data-stu-id="9cf32-595">The argument list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref` or `out`) of the corresponding parameter in the *formal_parameter_list* of `D`.</span></span>
    * <span data-ttu-id="9cf32-596">所考慮的候選方法是僅適用于其一般格式（適用函式[成員](expressions.md#applicable-function-member)）的方法，而不是只適用于其擴充形式的方法。</span><span class="sxs-lookup"><span data-stu-id="9cf32-596">The candidate methods considered are only those methods that are applicable in their normal form ([Applicable function member](expressions.md#applicable-function-member)), not those applicable only in their expanded form.</span></span>
*  <span data-ttu-id="9cf32-597">如果[方法調用](expressions.md#method-invocations)的演算法產生錯誤，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-597">If the algorithm of [Method invocations](expressions.md#method-invocations) produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="9cf32-598">否則，此演算法會產生一個最佳方法，`M` 具有與 `D` 相同數目的參數，並將轉換視為存在。</span><span class="sxs-lookup"><span data-stu-id="9cf32-598">Otherwise the algorithm produces a single best method `M` having the same number of parameters as `D` and the conversion is considered to exist.</span></span>
*  <span data-ttu-id="9cf32-599">選取的方法 `M` 必須與委派類型 `D`相容（[委派相容性](delegates.md#delegate-compatibility)），否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="9cf32-599">The selected method `M` must be compatible ([Delegate compatibility](delegates.md#delegate-compatibility)) with the delegate type `D`, or otherwise, a compile-time error occurs.</span></span>
*  <span data-ttu-id="9cf32-600">如果選取的方法 `M` 是實例方法，則與 `E` 相關聯的實例運算式會決定委派的目標物件。</span><span class="sxs-lookup"><span data-stu-id="9cf32-600">If the selected method `M` is an instance method, the instance expression associated with `E` determines the target object of the delegate.</span></span>
*  <span data-ttu-id="9cf32-601">如果選取的方法 M 是透過實例運算式上的成員存取所表示的擴充方法，則該實例運算式會決定委派的目標物件。</span><span class="sxs-lookup"><span data-stu-id="9cf32-601">If the selected method M is an extension method which is denoted by means of a member access on an instance expression, that instance expression determines the target object of the delegate.</span></span>
*  <span data-ttu-id="9cf32-602">轉換的結果是 `D`類型的值，也就是參考所選方法和目標物件的新建立委派。</span><span class="sxs-lookup"><span data-stu-id="9cf32-602">The result of the conversion is a value of type `D`, namely a newly created delegate that refers to the selected method and target object.</span></span>
*  <span data-ttu-id="9cf32-603">請注意，如果[方法調用](expressions.md#method-invocations)的演算法找不到實例方法，但卻成功將 `E(A)` 的調用當做擴充方法調用（[擴充方法](expressions.md#extension-method-invocations)調用）處理，則此程式可能會導致建立擴充方法的委派。</span><span class="sxs-lookup"><span data-stu-id="9cf32-603">Note that this process can lead to the creation of a delegate to an extension method, if the algorithm of [Method invocations](expressions.md#method-invocations) fails to find an instance method but succeeds in processing the invocation of `E(A)` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="9cf32-604">因此，建立的委派會捕捉擴充方法以及其第一個引數。</span><span class="sxs-lookup"><span data-stu-id="9cf32-604">A delegate thus created captures the extension method as well as its first argument.</span></span>

<span data-ttu-id="9cf32-605">下列範例示範方法群組轉換：</span><span class="sxs-lookup"><span data-stu-id="9cf32-605">The following example demonstrates method group conversions:</span></span>
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

<span data-ttu-id="9cf32-606">`d1` 的指派會將方法群組 `F` 隱含地轉換為 `D1`類型的值。</span><span class="sxs-lookup"><span data-stu-id="9cf32-606">The assignment to `d1` implicitly converts the method group `F` to a value of type `D1`.</span></span>

<span data-ttu-id="9cf32-607">[指派給 `d2`] 會顯示如何建立委派給較少衍生（逆變）參數類型的方法，以及更多衍生的（協變數）傳回型別。</span><span class="sxs-lookup"><span data-stu-id="9cf32-607">The assignment to `d2` shows how it is possible to create a delegate to a method that has less derived (contravariant) parameter types and a more derived (covariant) return type.</span></span>

<span data-ttu-id="9cf32-608">指派給 `d3` 會顯示如果方法不適用，則不會有任何轉換存在。</span><span class="sxs-lookup"><span data-stu-id="9cf32-608">The assignment to `d3` shows how no conversion exists if the method is not applicable.</span></span>

<span data-ttu-id="9cf32-609">[指派給 `d4`] 會顯示方法在其一般格式中的運作方式。</span><span class="sxs-lookup"><span data-stu-id="9cf32-609">The assignment to `d4` shows how the method must be applicable in its normal form.</span></span>

<span data-ttu-id="9cf32-610">`d5` 的指派會顯示委派和方法的參數和傳回型別，如何只針對引用型別而有所不同。</span><span class="sxs-lookup"><span data-stu-id="9cf32-610">The assignment to `d5` shows how parameter and return types of the delegate and method are allowed to differ only for reference types.</span></span>

<span data-ttu-id="9cf32-611">如同所有其他的隱含和明確轉換，cast 運算子可以用來明確地執行方法群組轉換。</span><span class="sxs-lookup"><span data-stu-id="9cf32-611">As with all other implicit and explicit conversions, the cast operator can be used to explicitly perform a method group conversion.</span></span> <span data-ttu-id="9cf32-612">因此，範例</span><span class="sxs-lookup"><span data-stu-id="9cf32-612">Thus, the example</span></span>
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
<span data-ttu-id="9cf32-613">可以改為寫入</span><span class="sxs-lookup"><span data-stu-id="9cf32-613">could instead be written</span></span>
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

<span data-ttu-id="9cf32-614">方法群組可能會影響多載解析，並參與型別推斷。</span><span class="sxs-lookup"><span data-stu-id="9cf32-614">Method groups may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="9cf32-615">如需進一步的詳細資料，請參閱[函數成員](expressions.md#function-members)。</span><span class="sxs-lookup"><span data-stu-id="9cf32-615">See [Function members](expressions.md#function-members) for further details.</span></span>

<span data-ttu-id="9cf32-616">方法群組轉換的執行時間評估會繼續進行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9cf32-616">The run-time evaluation of a method group conversion proceeds as follows:</span></span>

*  <span data-ttu-id="9cf32-617">如果在編譯時期選取的方法是實例方法，或者它是當做實例方法存取的擴充方法，則會從與 `E`相關聯的實例運算式來決定委派的目標物件：</span><span class="sxs-lookup"><span data-stu-id="9cf32-617">If the method selected at compile-time is an instance method, or it is an extension method which is accessed as an instance method, the target object of the delegate is determined from the instance expression associated with `E`:</span></span>
    * <span data-ttu-id="9cf32-618">實例運算式會進行評估。</span><span class="sxs-lookup"><span data-stu-id="9cf32-618">The instance expression is evaluated.</span></span> <span data-ttu-id="9cf32-619">如果此評估導致例外狀況，則不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="9cf32-619">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="9cf32-620">如果實例運算式是*reference_type*，實例運算式所計算的值就會成為目標物件。</span><span class="sxs-lookup"><span data-stu-id="9cf32-620">If the instance expression is of a *reference_type*, the value computed by the instance expression becomes the target object.</span></span> <span data-ttu-id="9cf32-621">如果選取的方法是實例方法，而目標物件是 `null`，就會擲回 `System.NullReferenceException`，而且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="9cf32-621">If the selected method is an instance method and the target object is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="9cf32-622">如果實例運算式是*value_type*，則會執行「裝箱」作業（「[裝箱」轉換](types.md#boxing-conversions)）來將值轉換為物件，而此物件會成為目標物件。</span><span class="sxs-lookup"><span data-stu-id="9cf32-622">If the instance expression is of a *value_type*, a boxing operation ([Boxing conversions](types.md#boxing-conversions)) is performed to convert the value to an object, and this object becomes the target object.</span></span>
*  <span data-ttu-id="9cf32-623">否則，選取的方法是靜態方法呼叫的一部分，而委派的目標物件則是 `null`。</span><span class="sxs-lookup"><span data-stu-id="9cf32-623">Otherwise the selected method is part of a static method call, and the target object of the delegate is `null`.</span></span>
*  <span data-ttu-id="9cf32-624">已配置 `D` 委派類型的新實例。</span><span class="sxs-lookup"><span data-stu-id="9cf32-624">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="9cf32-625">如果沒有足夠的記憶體可配置新的實例，則會擲回 `System.OutOfMemoryException`，且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="9cf32-625">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="9cf32-626">新的委派實例會使用在編譯時期決定的方法參考，以及上述所計算之目標物件的參考，進行初始化。</span><span class="sxs-lookup"><span data-stu-id="9cf32-626">The new delegate instance is initialized with a reference to the method that was determined at compile-time and a reference to the target object computed above.</span></span>
