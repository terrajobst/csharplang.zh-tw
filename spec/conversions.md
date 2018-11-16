# <a name="conversions"></a><span data-ttu-id="7c308-101">轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-101">Conversions</span></span>

<span data-ttu-id="7c308-102">A***轉換***可讓運算式被視為特定的型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-102">A ***conversion*** enables an expression to be treated as being of a particular type.</span></span> <span data-ttu-id="7c308-103">轉換可能會造成指定的型別被視為擁有不同類型的運算式，或可能會導致沒有取得型別之型別的運算式。</span><span class="sxs-lookup"><span data-stu-id="7c308-103">A conversion may cause an expression of a given type to be treated as having a different type, or it may cause an expression without a type to get a type.</span></span> <span data-ttu-id="7c308-104">轉換十分***隱含***或***明確***，這會決定是否需要明確轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-104">Conversions can be ***implicit*** or ***explicit***, and this determines whether an explicit cast is required.</span></span> <span data-ttu-id="7c308-105">比方說，從類型轉換`int`鍵入`long`是隱含的因此運算式的型別`int`隱含地視為類型`long`。</span><span class="sxs-lookup"><span data-stu-id="7c308-105">For instance, the conversion from type `int` to type `long` is implicit, so expressions of type `int` can implicitly be treated as type `long`.</span></span> <span data-ttu-id="7c308-106">相反的轉換，從型別`long`輸入`int`，是明確，因此需要明確轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-106">The opposite conversion, from type `long` to type `int`, is explicit and so an explicit cast is required.</span></span>

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

<span data-ttu-id="7c308-107">部分轉換是由語言定義。</span><span class="sxs-lookup"><span data-stu-id="7c308-107">Some conversions are defined by the language.</span></span> <span data-ttu-id="7c308-108">程式也可以定義自己的轉換 ([使用者定義轉換](conversions.md#user-defined-conversions))。</span><span class="sxs-lookup"><span data-stu-id="7c308-108">Programs may also define their own conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

## <a name="implicit-conversions"></a><span data-ttu-id="7c308-109">隱含轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-109">Implicit conversions</span></span>

<span data-ttu-id="7c308-110">下列轉換會歸類為隱含轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-110">The following conversions are classified as implicit conversions:</span></span>

*  <span data-ttu-id="7c308-111">身分識別轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-111">Identity conversions</span></span>
*  <span data-ttu-id="7c308-112">隱含數值轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-112">Implicit numeric conversions</span></span>
*  <span data-ttu-id="7c308-113">隱含的列舉型別轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-113">Implicit enumeration conversions.</span></span>
*  <span data-ttu-id="7c308-114">可為 null 的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-114">Implicit nullable conversions</span></span>
*  <span data-ttu-id="7c308-115">Null 常值轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-115">Null literal conversions</span></span>
*  <span data-ttu-id="7c308-116">隱含參考轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-116">Implicit reference conversions</span></span>
*  <span data-ttu-id="7c308-117">Boxing 轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-117">Boxing conversions</span></span>
*  <span data-ttu-id="7c308-118">動態的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-118">Implicit dynamic conversions</span></span>
*  <span data-ttu-id="7c308-119">隱含的常數運算式轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-119">Implicit constant expression conversions</span></span>
*  <span data-ttu-id="7c308-120">使用者定義的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-120">User-defined implicit conversions</span></span>
*  <span data-ttu-id="7c308-121">匿名函式轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-121">Anonymous function conversions</span></span>
*  <span data-ttu-id="7c308-122">方法群組轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-122">Method group conversions</span></span>

<span data-ttu-id="7c308-123">隱含轉換可能會發生各種不同的情況下，包括函式成員引動過程 ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution))，轉換運算式 ([轉型運算式](expressions.md#cast-expressions))，和指派 ([指派運算子](expressions.md#assignment-operators))。</span><span class="sxs-lookup"><span data-stu-id="7c308-123">Implicit conversions can occur in a variety of situations, including function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), and assignments ([Assignment operators](expressions.md#assignment-operators)).</span></span>

<span data-ttu-id="7c308-124">預先定義的隱含轉換永遠成功，而且永遠不會造成擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7c308-124">The pre-defined implicit conversions always succeed and never cause exceptions to be thrown.</span></span> <span data-ttu-id="7c308-125">設計得當的以使用者定義的隱含轉換，應該會表現出以及這些特性。</span><span class="sxs-lookup"><span data-stu-id="7c308-125">Properly designed user-defined implicit conversions should exhibit these characteristics as well.</span></span>

<span data-ttu-id="7c308-126">類型的轉換，基於`object`和`dynamic`都視為相等。</span><span class="sxs-lookup"><span data-stu-id="7c308-126">For the purposes of conversion, the types `object` and `dynamic` are considered equivalent.</span></span>

<span data-ttu-id="7c308-127">不過，動態轉換 ([動態的隱含轉換](conversions.md#implicit-dynamic-conversions)並[明確的動態轉換](conversions.md#explicit-dynamic-conversions)) 只適用於運算式的型別`dynamic`([動態型別](types.md#the-dynamic-type)).</span><span class="sxs-lookup"><span data-stu-id="7c308-127">However, dynamic conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)) apply only to expressions of type `dynamic` ([The dynamic type](types.md#the-dynamic-type)).</span></span>

### <a name="identity-conversion"></a><span data-ttu-id="7c308-128">身分識別轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-128">Identity conversion</span></span>

<span data-ttu-id="7c308-129">身分識別轉換將從任何類型轉換成相同的型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-129">An identity conversion converts from any type to the same type.</span></span> <span data-ttu-id="7c308-130">這項轉換存在，可說是實體已經有必要的型別可轉換成該類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-130">This conversion exists such that an entity that already has a required type can be said to be convertible to that type.</span></span>

*  <span data-ttu-id="7c308-131">因為物件及動態都視為相等，所以沒有身分識別轉換之間`object`及`dynamic`，並取代所有出現時，都是相同的建構型別之間`dynamic`與`object`。</span><span class="sxs-lookup"><span data-stu-id="7c308-131">Because object and dynamic are considered equivalent there is an identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing all occurrences of `dynamic` with `object`.</span></span>

### <a name="implicit-numeric-conversions"></a><span data-ttu-id="7c308-132">隱含數值轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-132">Implicit numeric conversions</span></span>

<span data-ttu-id="7c308-133">隱含數值轉換為：</span><span class="sxs-lookup"><span data-stu-id="7c308-133">The implicit numeric conversions are:</span></span>

*  <span data-ttu-id="7c308-134">從`sbyte`要`short`， `int`， `long`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-134">From `sbyte` to `short`, `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-135">從`byte`要`short`， `ushort`， `int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-135">From `byte` to `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-136">從`short`要`int`， `long`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-136">From `short` to `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-137">從`ushort`要`int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-137">From `ushort` to `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-138">從`int`要`long`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-138">From `int` to `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-139">從`uint`要`long`， `ulong`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-139">From `uint` to `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-140">從`long`要`float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-140">From `long` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-141">從`ulong`要`float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-141">From `ulong` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-142">從`char`要`ushort`， `int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-142">From `char` to `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-143">從`float`至`double`。</span><span class="sxs-lookup"><span data-stu-id="7c308-143">From `float` to `double`.</span></span>

<span data-ttu-id="7c308-144">從不`int`， `uint`， `long`，或`ulong`來`float`進出`long`或`ulong`至`double`可能會導致失去精確度，但永遠不會造成又級距性遺失。</span><span class="sxs-lookup"><span data-stu-id="7c308-144">Conversions from `int`, `uint`, `long`, or `ulong` to `float` and from `long` or `ulong` to `double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="7c308-145">其他隱含數值轉換不會遺失任何資訊。</span><span class="sxs-lookup"><span data-stu-id="7c308-145">The other implicit numeric conversions never lose any information.</span></span>

<span data-ttu-id="7c308-146">有沒有隱含轉換`char`型別，因此其他整數類資料類型的值無法自動轉換成`char`型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-146">There are no implicit conversions to the `char` type, so values of the other integral types do not automatically convert to the `char` type.</span></span>

### <a name="implicit-enumeration-conversions"></a><span data-ttu-id="7c308-147">隱含的列舉型別轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-147">Implicit enumeration conversions</span></span>

<span data-ttu-id="7c308-148">允許隱含的列舉型別轉換*decimal_integer_literal* `0`轉換成任何*enum_type*和任何*nullable_type*其基礎類型是*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-148">An implicit enumeration conversion permits the *decimal_integer_literal* `0` to be converted to any *enum_type* and to any *nullable_type* whose underlying type is an *enum_type*.</span></span> <span data-ttu-id="7c308-149">在後者的情況，則轉換會評估轉換至基礎*enum_type*換行的結果 ([可為 Null 的型別](types.md#nullable-types))。</span><span class="sxs-lookup"><span data-stu-id="7c308-149">In the latter case the conversion is evaluated by converting to the underlying *enum_type* and wrapping the result ([Nullable types](types.md#nullable-types)).</span></span>

### <a name="implicit-interpolated-string-conversions"></a><span data-ttu-id="7c308-150">隱含的字串插值的轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-150">Implicit interpolated string conversions</span></span>

<span data-ttu-id="7c308-151">隱含的插補字串轉換允許*interpolated_string_expression* ([插補字串](expressions.md#interpolated-strings)) 轉換成`System.IFormattable`或`System.FormattableString`（它會實作`System.IFormattable`).</span><span class="sxs-lookup"><span data-stu-id="7c308-151">An implicit interpolated string conversion permits an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)) to be converted to `System.IFormattable` or `System.FormattableString` (which implements `System.IFormattable`).</span></span>

<span data-ttu-id="7c308-152">當套用這項轉換的字串值不被由字串插值。</span><span class="sxs-lookup"><span data-stu-id="7c308-152">When this conversion is applied a string value is not composed from the interpolated string.</span></span> <span data-ttu-id="7c308-153">改為執行個體`System.FormattableString`是建立中，將將進一步說明[插補字串](expressions.md#interpolated-strings)。</span><span class="sxs-lookup"><span data-stu-id="7c308-153">Instead an instance of `System.FormattableString` is created, as further described in [Interpolated strings](expressions.md#interpolated-strings).</span></span>

### <a name="implicit-nullable-conversions"></a><span data-ttu-id="7c308-154">可為 null 的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-154">Implicit nullable conversions</span></span>

<span data-ttu-id="7c308-155">預先定義的隱含轉換對不可為 null 的實值型別也可以搭配這些類型可為 null 的表單。</span><span class="sxs-lookup"><span data-stu-id="7c308-155">Predefined implicit conversions that operate on non-nullable value types can also be used with nullable forms of those types.</span></span> <span data-ttu-id="7c308-156">針對每個預先定義隱含的身分識別和從非可為 null 的實值型別轉換的數值轉換`S`不可為 null 的實值型別的`T`，下列隱含可為 null 的轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-156">For each of the predefined implicit identity and numeric conversions that convert from a non-nullable value type `S` to a non-nullable value type `T`, the following implicit nullable conversions exist:</span></span>

*  <span data-ttu-id="7c308-157">從的隱含轉換`S?`至`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-157">An implicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="7c308-158">從的隱含轉換`S`至`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-158">An implicit conversion from `S` to `T?`.</span></span>

<span data-ttu-id="7c308-159">評估可為 null 的隱含轉換為基礎的基礎轉換`S`至`T`程序如下：</span><span class="sxs-lookup"><span data-stu-id="7c308-159">Evaluation of an implicit nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="7c308-160">如果可為 null 的轉換是來自`S?`至`T?`:</span><span class="sxs-lookup"><span data-stu-id="7c308-160">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="7c308-161">如果來源值為 null (`HasValue`屬性為 false)，結果就是 null 值的型別`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-161">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="7c308-162">否則，則轉換會評估為從`S?`來`S`，後面接著從基礎轉換`S`來`T`，後面接著換行 ([可為 Null 的型別](types.md#nullable-types)) 從`T`至`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-162">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping ([Nullable types](types.md#nullable-types)) from `T` to `T?`.</span></span>

*  <span data-ttu-id="7c308-163">如果可為 null 的轉換是來自`S`來`T?`，則轉換會評估為基礎的轉換，從`S`來`T`後面接著從繞`T`來`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-163">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>

### <a name="null-literal-conversions"></a><span data-ttu-id="7c308-164">Null 常值轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-164">Null literal conversions</span></span>

<span data-ttu-id="7c308-165">隱含的轉換`null`常值至任何可為 null 的型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-165">An implicit conversion exists from the `null` literal to any nullable type.</span></span> <span data-ttu-id="7c308-166">這項轉換會產生 null 值 ([可為 Null 的型別](types.md#nullable-types)) 指定可為 null 的類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-166">This conversion produces the null value ([Nullable types](types.md#nullable-types)) of the given nullable type.</span></span>

### <a name="implicit-reference-conversions"></a><span data-ttu-id="7c308-167">隱含參考轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-167">Implicit reference conversions</span></span>

<span data-ttu-id="7c308-168">隱含參考轉換為：</span><span class="sxs-lookup"><span data-stu-id="7c308-168">The implicit reference conversions are:</span></span>

*  <span data-ttu-id="7c308-169">從任何*reference_type*要`object`和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="7c308-169">From any *reference_type* to `object` and `dynamic`.</span></span>
*  <span data-ttu-id="7c308-170">從任何*class_type* `S`任何*class_type* `T`提供`S`衍生自`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-170">From any *class_type* `S` to any *class_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="7c308-171">從任何*class_type* `S`任何*interface_type* `T`提供`S`實作`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-171">From any *class_type* `S` to any *interface_type* `T`, provided `S` implements `T`.</span></span>
*  <span data-ttu-id="7c308-172">從任何*interface_type* `S`任何*interface_type* `T`提供`S`衍生自`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-172">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="7c308-173">從*array_type* `S`項目類型`SE`來*array_type* `T`項目類型`TE`，前提是所有下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="7c308-173">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="7c308-174">`S` 和`T`的差別只在於項目型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-174">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="7c308-175">亦即`S`和`T`有相同的維度數目。</span><span class="sxs-lookup"><span data-stu-id="7c308-175">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="7c308-176">兩者`SE`並`TE`會*reference_type*s。</span><span class="sxs-lookup"><span data-stu-id="7c308-176">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="7c308-177">隱含參考轉換存在`SE`至`TE`。</span><span class="sxs-lookup"><span data-stu-id="7c308-177">An implicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="7c308-178">從任何*array_type*到`System.Array`和它所實作的介面。</span><span class="sxs-lookup"><span data-stu-id="7c308-178">From any *array_type* to `System.Array` and the interfaces it implements.</span></span>
*  <span data-ttu-id="7c308-179">從一維陣列的型別`S[]`要`System.Collections.Generic.IList<T>`和其基底介面，提供從隱含身分識別或參考轉換`S`至`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-179">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an implicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="7c308-180">從任何*delegate_type*到`System.Delegate`和它所實作的介面。</span><span class="sxs-lookup"><span data-stu-id="7c308-180">From any *delegate_type* to `System.Delegate` and the interfaces it implements.</span></span>
*  <span data-ttu-id="7c308-181">從任何 null 常值*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-181">From the null literal to any *reference_type*.</span></span>
*  <span data-ttu-id="7c308-182">從任何*reference_type*要*reference_type* `T`有隱含的身分識別或參考轉換為*reference_type* `T0`和`T0`具有身分識別轉換至`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-182">From any *reference_type* to a *reference_type* `T` if it has an implicit identity or reference conversion to a *reference_type* `T0` and `T0` has an identity conversion to `T`.</span></span>
*  <span data-ttu-id="7c308-183">從任何*reference_type*介面或委派型別的`T`有隱含的身分識別或參考轉換為介面或委派的型別`T0`和`T0`可轉換變異數 ([變異數轉換](interfaces.md#variance-conversion)) 以`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-183">From any *reference_type* to an interface or delegate type `T` if it has an implicit identity or reference conversion to an interface or delegate type `T0` and `T0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `T`.</span></span>
*  <span data-ttu-id="7c308-184">涉及為參考型別的已知型別參數的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-184">Implicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="7c308-185">請參閱[涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)如需詳細資訊，涉及型別參數的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-185">See [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) for more details on implicit conversions involving type parameters.</span></span>

<span data-ttu-id="7c308-186">隱含參考轉換是指在轉換*reference_type*s 可證實為一律會成功，並因此需要在執行階段沒有任何檢查。</span><span class="sxs-lookup"><span data-stu-id="7c308-186">The implicit reference conversions are those conversions between *reference_type*s that can be proven to always succeed, and therefore require no checks at run-time.</span></span>

<span data-ttu-id="7c308-187">參考轉換，隱含或明確的永遠不會變更轉換的物件參考的識別。</span><span class="sxs-lookup"><span data-stu-id="7c308-187">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="7c308-188">換句話說，雖然參考轉換可能會變更參考的類型，但它絕不會變更型別或所參考之物件的值。</span><span class="sxs-lookup"><span data-stu-id="7c308-188">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="7c308-189">Boxing 轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-189">Boxing conversions</span></span>

<span data-ttu-id="7c308-190">Boxing 轉換允許*value_type*隱含地轉換成參考型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-190">A boxing conversion permits a *value_type* to be implicitly converted to a reference type.</span></span> <span data-ttu-id="7c308-191">Boxing 轉換存在從任何*non_nullable_value_type*要`object`並`dynamic`至`System.ValueType`和任何*interface_type*藉由將*non_nullable_value_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-191">A boxing conversion exists from any *non_nullable_value_type* to `object` and `dynamic`, to `System.ValueType` and to any *interface_type* implemented by the *non_nullable_value_type*.</span></span> <span data-ttu-id="7c308-192">此外*enum_type*可以轉換成類型`System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="7c308-192">Furthermore an *enum_type* can be converted to the type `System.Enum`.</span></span>

<span data-ttu-id="7c308-193">Boxing 轉換存在*nullable_type*為參考型別，如果且只有 boxing 轉換存在從基礎*non_nullable_value_type*參考類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-193">A boxing conversion exists from a *nullable_type* to a reference type, if and only if a boxing conversion exists from the underlying *non_nullable_value_type* to the reference type.</span></span>

<span data-ttu-id="7c308-194">實值型別具有介面類型的 boxing 轉換`I`是否為介面類型的 boxing 轉換`I0`並`I0`具有身分識別轉換至`I`。</span><span class="sxs-lookup"><span data-stu-id="7c308-194">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="7c308-195">實值型別具有介面類型的 boxing 轉換`I`有介面或委派型別的 boxing 轉換`I0`並`I0`可轉換變異數 ([變異數轉換](interfaces.md#variance-conversion)) 到`I`.</span><span class="sxs-lookup"><span data-stu-id="7c308-195">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface or delegate type `I0` and `I0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `I`.</span></span>

<span data-ttu-id="7c308-196">Box 處理的值*non_nullable_value_type*配置的物件執行個體，並複製所組成*value_type*到該執行個體的值。</span><span class="sxs-lookup"><span data-stu-id="7c308-196">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *value_type* value into that instance.</span></span> <span data-ttu-id="7c308-197">結構可以成為 boxed 型別`System.ValueType`，因為這是全部為結構的基底類別 ([繼承](structs.md#inheritance))。</span><span class="sxs-lookup"><span data-stu-id="7c308-197">A struct can be boxed to the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="7c308-198">Box 處理的值*nullable_type*程序如下：</span><span class="sxs-lookup"><span data-stu-id="7c308-198">Boxing a value of a *nullable_type* proceeds as follows:</span></span>

*  <span data-ttu-id="7c308-199">如果來源值為 null (`HasValue`屬性為 false)，結果為 null 參考的目標型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-199">If the source value is null (`HasValue` property is false), the result is a null reference of the target type.</span></span>
*  <span data-ttu-id="7c308-200">否則，結果就是參考 boxed 的`T`解除包裝成為和 boxing 來源值產生。</span><span class="sxs-lookup"><span data-stu-id="7c308-200">Otherwise, the result is a reference to a boxed `T` produced by unwrapping and boxing the source value.</span></span>

<span data-ttu-id="7c308-201">Boxing 轉換中所述進一步[Boxing 轉換](types.md#boxing-conversions)。</span><span class="sxs-lookup"><span data-stu-id="7c308-201">Boxing conversions are described further in [Boxing conversions](types.md#boxing-conversions).</span></span>

### <a name="implicit-dynamic-conversions"></a><span data-ttu-id="7c308-202">動態的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-202">Implicit dynamic conversions</span></span>

<span data-ttu-id="7c308-203">隱含的動態轉換存在類型的運算式`dynamic`任何型別的`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-203">An implicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="7c308-204">轉換動態繫結 ([動態繫結](expressions.md#dynamic-binding))，這表示，會在執行階段從執行階段類型的運算式來搜尋的隱含轉換`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-204">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an implicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="7c308-205">如果不找到任何轉換，則會擲回執行階段例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7c308-205">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="7c308-206">請注意這項隱含轉換看似違反開頭的建議[隱含轉換](conversions.md#implicit-conversions)隱含的轉換應該永遠不會造成例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7c308-206">Note that this implicit conversion seemingly violates the advice in the beginning of [Implicit conversions](conversions.md#implicit-conversions) that an implicit conversion should never cause an exception.</span></span> <span data-ttu-id="7c308-207">不過它不是轉換本身，但*尋找*造成例外狀況的轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-207">However it is not the conversion itself, but the *finding* of the conversion that causes the exception.</span></span> <span data-ttu-id="7c308-208">執行階段例外狀況的風險是可繼承的動態繫結使用。</span><span class="sxs-lookup"><span data-stu-id="7c308-208">The risk of run-time exceptions is inherent in the use of dynamic binding.</span></span> <span data-ttu-id="7c308-209">如果不需要轉換的動態繫結，運算式可以先轉換成`object`，然後將所需的類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-209">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="7c308-210">下列範例說明隱含的動態轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-210">The following example illustrates implicit dynamic conversions:</span></span>

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

<span data-ttu-id="7c308-211">若要指派`s2`和`i`同時採用隱含動態轉換作業的繫結中暫止執行階段。</span><span class="sxs-lookup"><span data-stu-id="7c308-211">The assignments to `s2` and `i` both employ implicit dynamic conversions, where the binding of the operations is suspended until run-time.</span></span> <span data-ttu-id="7c308-212">在執行階段隱含轉換為搜尋的執行階段型別`d`  --  `string` -成目標型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-212">At run-time, implicit conversions are sought from the run-time type of `d` -- `string` -- to the target type.</span></span> <span data-ttu-id="7c308-213">轉換發現`string`而無法供`int`。</span><span class="sxs-lookup"><span data-stu-id="7c308-213">A conversion is found to `string` but not to `int`.</span></span>

### <a name="implicit-constant-expression-conversions"></a><span data-ttu-id="7c308-214">隱含的常數運算式轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-214">Implicit constant expression conversions</span></span>

<span data-ttu-id="7c308-215">隱含的常數運算式允許下列轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-215">An implicit constant expression conversion permits the following conversions:</span></span>

*  <span data-ttu-id="7c308-216">A *constant_expression* ([常數運算式](expressions.md#constant-expressions)) 的型別`int`可以轉換為類型`sbyte`， `byte`， `short`， `ushort`， `uint`，或`ulong`，提供的值*constant_expression*目的地類型的範圍內。</span><span class="sxs-lookup"><span data-stu-id="7c308-216">A *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) of type `int` can be converted to type `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the *constant_expression* is within the range of the destination type.</span></span>
*  <span data-ttu-id="7c308-217">A *constant_expression*型別的`long`可以轉換為類型`ulong`，提供的值*constant_expression*不是負數。</span><span class="sxs-lookup"><span data-stu-id="7c308-217">A *constant_expression* of type `long` can be converted to type `ulong`, provided the value of the *constant_expression* is not negative.</span></span>

### <a name="implicit-conversions-involving-type-parameters"></a><span data-ttu-id="7c308-218">包含型別參數的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-218">Implicit conversions involving type parameters</span></span>

<span data-ttu-id="7c308-219">下列的隱含轉換存在指定的型別參數`T`:</span><span class="sxs-lookup"><span data-stu-id="7c308-219">The following implicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="7c308-220">從`T`其有效的基底類別`C`，從`T`任何基底類別`C`，以及從`T`實作任何介面`C`。</span><span class="sxs-lookup"><span data-stu-id="7c308-220">From `T` to its effective base class `C`, from `T` to any base class of `C`, and from `T` to any interface implemented by `C`.</span></span> <span data-ttu-id="7c308-221">在執行階段，如果`T`是實值類型，轉換會執行 boxing 轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-221">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="7c308-222">否則，轉換會執行隱含參考轉換或識別轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-222">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="7c308-223">從`T`為介面型別`I`中`T`設定有效的介面以及從`T`任何基底介面`I`。</span><span class="sxs-lookup"><span data-stu-id="7c308-223">From `T` to an interface type `I` in `T`'s effective interface set and from `T` to any base interface of `I`.</span></span> <span data-ttu-id="7c308-224">在執行階段，如果`T`是實值類型，轉換會執行 boxing 轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-224">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="7c308-225">否則，轉換會執行隱含參考轉換或識別轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-225">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="7c308-226">從`T`類型參數`U`提供`T`取決於`U`([類型參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="7c308-226">From `T` to a type parameter `U`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="7c308-227">在執行階段，如果`U`是實值類型，則`T`和`U`一定都是相同的型別並不會執行轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-227">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="7c308-228">否則，如果`T`是實值類型，轉換會執行 boxing 轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-228">Otherwise, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="7c308-229">否則，轉換會執行隱含參考轉換或識別轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-229">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="7c308-230">Null 常值，以便從`T`提供`T`已知為參考型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-230">From the null literal to `T`, provided `T` is known to be a reference type.</span></span>
*  <span data-ttu-id="7c308-231">從`T`參考型別`I`是否有參考類型的隱含轉換`S0`並`S0`具有身分識別轉換至`S`。</span><span class="sxs-lookup"><span data-stu-id="7c308-231">From `T` to a reference type `I` if it has an implicit conversion to a reference type `S0` and `S0` has an identity conversion to `S`.</span></span> <span data-ttu-id="7c308-232">轉換會在執行階段執行內容轉換成一樣`S0`。</span><span class="sxs-lookup"><span data-stu-id="7c308-232">At run-time the conversion is executed the same way as the conversion to `S0`.</span></span>
*  <span data-ttu-id="7c308-233">從`T`為介面型別`I`有隱含轉換成介面或委派的型別`I0`並`I0`可轉換變異數來`I`([變異數轉換](interfaces.md#variance-conversion)).</span><span class="sxs-lookup"><span data-stu-id="7c308-233">From `T` to an interface type `I` if it has an implicit conversion to an interface or delegate type `I0` and `I0` is variance-convertible to `I` ([Variance conversion](interfaces.md#variance-conversion)).</span></span> <span data-ttu-id="7c308-234">在執行階段，如果`T`是實值類型，轉換會執行 boxing 轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-234">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="7c308-235">否則，轉換會執行隱含參考轉換或識別轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-235">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="7c308-236">如果`T`已知為參考型別 ([類型參數條件約束](classes.md#type-parameter-constraints))，則上述的轉換會歸類為隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="7c308-236">If `T` is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)), the conversions above are all classified as implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="7c308-237">如果`T`不是知道是參考類型，則上述轉換歸類為 boxing 轉換 ([Boxing 轉換](conversions.md#boxing-conversions))。</span><span class="sxs-lookup"><span data-stu-id="7c308-237">If `T` is not known to be a reference type, the conversions above are classified as boxing conversions ([Boxing conversions](conversions.md#boxing-conversions)).</span></span>

### <a name="user-defined-implicit-conversions"></a><span data-ttu-id="7c308-238">使用者定義的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-238">User-defined implicit conversions</span></span>

<span data-ttu-id="7c308-239">使用者定義的隱含轉換是由選擇性標準隱含的轉換，後面接著執行使用者定義的隱含轉換運算子，後面接著另一個選用標準的隱含轉換所組成。</span><span class="sxs-lookup"><span data-stu-id="7c308-239">A user-defined implicit conversion consists of an optional standard implicit conversion, followed by execution of a user-defined implicit conversion operator, followed by another optional standard implicit conversion.</span></span> <span data-ttu-id="7c308-240">評估使用者定義的隱含轉換的確切規則所述[處理的使用者定義的隱含轉換](conversions.md#processing-of-user-defined-implicit-conversions)。</span><span class="sxs-lookup"><span data-stu-id="7c308-240">The exact rules for evaluating user-defined implicit conversions are described in [Processing of user-defined implicit conversions](conversions.md#processing-of-user-defined-implicit-conversions).</span></span>

### <a name="anonymous-function-conversions-and-method-group-conversions"></a><span data-ttu-id="7c308-241">匿名函式轉換和方法群組轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-241">Anonymous function conversions and method group conversions</span></span>

<span data-ttu-id="7c308-242">匿名函式和方法群組沒有型別本身，但可能會隱含地轉換成委派類型或運算式樹狀架構類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-242">Anonymous functions and method groups do not have types in and of themselves, but may be implicitly converted to delegate types or expression tree types.</span></span> <span data-ttu-id="7c308-243">更詳細地說明匿名函式轉換[匿名函式轉換](conversions.md#anonymous-function-conversions)和中的方法群組轉換[方法群組轉換](conversions.md#method-group-conversions)。</span><span class="sxs-lookup"><span data-stu-id="7c308-243">Anonymous function conversions are described in more detail in [Anonymous function conversions](conversions.md#anonymous-function-conversions) and method group conversions in [Method group conversions](conversions.md#method-group-conversions).</span></span>

## <a name="explicit-conversions"></a><span data-ttu-id="7c308-244">明確轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-244">Explicit conversions</span></span>

<span data-ttu-id="7c308-245">下列轉換會歸類為明確的轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-245">The following conversions are classified as explicit conversions:</span></span>

*  <span data-ttu-id="7c308-246">所有的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-246">All implicit conversions.</span></span>
*  <span data-ttu-id="7c308-247">明確數值轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-247">Explicit numeric conversions.</span></span>
*  <span data-ttu-id="7c308-248">明確的列舉型別轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-248">Explicit enumeration conversions.</span></span>
*  <span data-ttu-id="7c308-249">明確可為 null 的轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-249">Explicit nullable conversions.</span></span>
*  <span data-ttu-id="7c308-250">明確參考轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-250">Explicit reference conversions.</span></span>
*  <span data-ttu-id="7c308-251">明確介面轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-251">Explicit interface conversions.</span></span>
*  <span data-ttu-id="7c308-252">Unboxing 轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-252">Unboxing conversions.</span></span>
*  <span data-ttu-id="7c308-253">明確的動態轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-253">Explicit dynamic conversions</span></span>
*  <span data-ttu-id="7c308-254">使用者定義的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-254">User-defined explicit conversions.</span></span>

<span data-ttu-id="7c308-255">明確轉換可能會發生在轉型運算式 ([轉型運算式](expressions.md#cast-expressions))。</span><span class="sxs-lookup"><span data-stu-id="7c308-255">Explicit conversions can occur in cast expressions ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="7c308-256">明確轉換一組包含所有的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-256">The set of explicit conversions includes all implicit conversions.</span></span> <span data-ttu-id="7c308-257">這表示可以重複轉型運算式。</span><span class="sxs-lookup"><span data-stu-id="7c308-257">This means that redundant cast expressions are allowed.</span></span>

<span data-ttu-id="7c308-258">跨網域的差異足以值得一讀明確的型別不是隱含轉換的明確轉換是無法證明一律會成功的轉換，可能會遺失資訊，已知的轉換和轉換標記法。</span><span class="sxs-lookup"><span data-stu-id="7c308-258">The explicit conversions that are not implicit conversions are conversions that cannot be proven to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit explicit notation.</span></span>

### <a name="explicit-numeric-conversions"></a><span data-ttu-id="7c308-259">明確數值轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-259">Explicit numeric conversions</span></span>

<span data-ttu-id="7c308-260">明確數值轉換是從不*numeric_type*到另一個*numeric_type* ，隱含數值轉換 ([隱含數值轉換](conversions.md#implicit-numeric-conversions))不存在：</span><span class="sxs-lookup"><span data-stu-id="7c308-260">The explicit numeric conversions are the conversions from a *numeric_type* to another *numeric_type* for which an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) does not already exist:</span></span>

*  <span data-ttu-id="7c308-261">從`sbyte`要`byte`， `ushort`， `uint`， `ulong`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="7c308-261">From `sbyte` to `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="7c308-262">從`byte`要`sbyte`和`char`。</span><span class="sxs-lookup"><span data-stu-id="7c308-262">From `byte` to `sbyte` and `char`.</span></span>
*  <span data-ttu-id="7c308-263">從`short`要`sbyte`， `byte`， `ushort`， `uint`， `ulong`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="7c308-263">From `short` to `sbyte`, `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="7c308-264">從`ushort`要`sbyte`， `byte`， `short`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="7c308-264">From `ushort` to `sbyte`, `byte`, `short`, or `char`.</span></span>
*  <span data-ttu-id="7c308-265">從`int`要`sbyte`， `byte`， `short`， `ushort`， `uint`， `ulong`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="7c308-265">From `int` to `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="7c308-266">從`uint`要`sbyte`， `byte`， `short`， `ushort`， `int`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="7c308-266">From `uint` to `sbyte`, `byte`, `short`, `ushort`, `int`, or `char`.</span></span>
*  <span data-ttu-id="7c308-267">從`long`要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `ulong`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="7c308-267">From `long` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="7c308-268">從`ulong`要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="7c308-268">From `ulong` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `char`.</span></span>
*  <span data-ttu-id="7c308-269">從`char`要`sbyte`， `byte`，或`short`。</span><span class="sxs-lookup"><span data-stu-id="7c308-269">From `char` to `sbyte`, `byte`, or `short`.</span></span>
*  <span data-ttu-id="7c308-270">從`float`要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-270">From `float` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-271">從`double`要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-271">From `double` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-272">從`decimal`要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`，或`double`。</span><span class="sxs-lookup"><span data-stu-id="7c308-272">From `decimal` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `double`.</span></span>

<span data-ttu-id="7c308-273">因為明確的轉換會包括所有隱含和明確數值轉換，它就一定能夠從任何轉換*numeric_type*任何其他*numeric_type*使用 cast 運算式 （[轉型運算式](expressions.md#cast-expressions))。</span><span class="sxs-lookup"><span data-stu-id="7c308-273">Because the explicit conversions include all implicit and explicit numeric conversions, it is always possible to convert from any *numeric_type* to any other *numeric_type* using a cast expression ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="7c308-274">明確數值轉換可能會遺失資訊，或可能會導致擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7c308-274">The explicit numeric conversions possibly lose information or possibly cause exceptions to be thrown.</span></span> <span data-ttu-id="7c308-275">明確數值轉換的處理方式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7c308-275">An explicit numeric conversion is processed as follows:</span></span>

*  <span data-ttu-id="7c308-276">從整數類資料類型轉換為另一種整數類資料類型，處理取決於溢位檢查內容 ([checked 與 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)) 會轉換在放置：</span><span class="sxs-lookup"><span data-stu-id="7c308-276">For a conversion from an integral type to another integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="7c308-277">在 `checked`內容，轉換成功，如果來源運算元的值範圍內的目的型別，但會擲回`System.OverflowException`若來源運算元的值超出目的地類型的範圍。</span><span class="sxs-lookup"><span data-stu-id="7c308-277">In a `checked` context, the conversion succeeds if the value of the source operand is within the range of the destination type, but throws a `System.OverflowException` if the value of the source operand is outside the range of the destination type.</span></span>
    * <span data-ttu-id="7c308-278">在 `unchecked`內容，轉換一律會成功，然後繼續進行，如下所示。</span><span class="sxs-lookup"><span data-stu-id="7c308-278">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="7c308-279">如果來源類型大於目的地類型，會藉由捨棄其「額外」最高有效位元，來截斷來源值。</span><span class="sxs-lookup"><span data-stu-id="7c308-279">If the source type is larger than the destination type, then the source value is truncated by discarding its "extra" most significant bits.</span></span> <span data-ttu-id="7c308-280">然後會將結果視為目標類型的值。</span><span class="sxs-lookup"><span data-stu-id="7c308-280">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="7c308-281">如果來源類型小於目標類型，則來源值可以是正負號擴充或零擴充，以使其與目標類型的大小相同。</span><span class="sxs-lookup"><span data-stu-id="7c308-281">If the source type is smaller than the destination type, then the source value is either sign-extended or zero-extended so that it is the same size as the destination type.</span></span> <span data-ttu-id="7c308-282">如果來源類型帶正負號，則會使用正負號擴充；如果來源類型不帶正負號，則會使用零擴充。</span><span class="sxs-lookup"><span data-stu-id="7c308-282">Sign-extension is used if the source type is signed; zero-extension is used if the source type is unsigned.</span></span> <span data-ttu-id="7c308-283">然後會將結果視為目標類型的值。</span><span class="sxs-lookup"><span data-stu-id="7c308-283">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="7c308-284">如果來源類型與目標類型的大小相同，則來源值將視為目標類型的值。</span><span class="sxs-lookup"><span data-stu-id="7c308-284">If the source type is the same size as the destination type, then the source value is treated as a value of the destination type.</span></span>
*  <span data-ttu-id="7c308-285">從轉換`decimal`為整數類型，來源值會捨入為最接近的整數值零，而此整數的值會變成轉換的結果。</span><span class="sxs-lookup"><span data-stu-id="7c308-285">For a conversion from `decimal` to an integral type, the source value is rounded towards zero to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="7c308-286">如果產生的整數值超出目的地類型中，各種`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="7c308-286">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="7c308-287">從轉換`float`或是`double`為整數類型，處理需視溢位檢查內容 ([checked 與 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)) 在會轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-287">For a conversion from `float` or `double` to an integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="7c308-288">在 `checked`內容，轉換程序如下：</span><span class="sxs-lookup"><span data-stu-id="7c308-288">In a `checked` context, the conversion proceeds as follows:</span></span>
        * <span data-ttu-id="7c308-289">如果運算元的值是 NaN 或 infinite，`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="7c308-289">If the value of the operand is NaN or infinite, a `System.OverflowException` is thrown.</span></span>
        * <span data-ttu-id="7c308-290">否則，來源運算元會捨入為最接近的整數值零。</span><span class="sxs-lookup"><span data-stu-id="7c308-290">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="7c308-291">如果此整數的值為目的地類型的範圍內這個值會是轉換的結果。</span><span class="sxs-lookup"><span data-stu-id="7c308-291">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="7c308-292">否則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="7c308-292">Otherwise, a `System.OverflowException` is thrown.</span></span>
    * <span data-ttu-id="7c308-293">在 `unchecked`內容，轉換一律會成功，然後繼續進行，如下所示。</span><span class="sxs-lookup"><span data-stu-id="7c308-293">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="7c308-294">如果運算元的值是否為 NaN 或無限，轉換的結果是未指定的目的地類型的值。</span><span class="sxs-lookup"><span data-stu-id="7c308-294">If the value of the operand is NaN or infinite, the result of the conversion is an unspecified value of the destination type.</span></span>
        * <span data-ttu-id="7c308-295">否則，來源運算元會捨入為最接近的整數值零。</span><span class="sxs-lookup"><span data-stu-id="7c308-295">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="7c308-296">如果此整數的值為目的地類型的範圍內這個值會是轉換的結果。</span><span class="sxs-lookup"><span data-stu-id="7c308-296">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="7c308-297">否則，轉換的結果就是目的地類型的未指定的值。</span><span class="sxs-lookup"><span data-stu-id="7c308-297">Otherwise, the result of the conversion is an unspecified value of the destination type.</span></span>
*  <span data-ttu-id="7c308-298">從轉換`double`要`float`，則`double`值會四捨五入為最接近`float`值。</span><span class="sxs-lookup"><span data-stu-id="7c308-298">For a conversion from `double` to `float`, the `double` value is rounded to the nearest `float` value.</span></span> <span data-ttu-id="7c308-299">如果`double`值太小而無法表示為`float`，結果變成正零或負零。</span><span class="sxs-lookup"><span data-stu-id="7c308-299">If the `double` value is too small to represent as a `float`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="7c308-300">如果`double`值太大而無法表示為`float`，結果會成為無限大的正數或負的無限值。</span><span class="sxs-lookup"><span data-stu-id="7c308-300">If the `double` value is too large to represent as a `float`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="7c308-301">如果`double`值是 NaN，結果也是 NaN。</span><span class="sxs-lookup"><span data-stu-id="7c308-301">If the `double` value is NaN, the result is also NaN.</span></span>
*  <span data-ttu-id="7c308-302">從轉換`float`或`double`來`decimal`，來源值轉換成`decimal`表示法和捨入到最接近的數字第 28 小數點之後如有必要 ([decimal 型別](types.md#the-decimal-type)).</span><span class="sxs-lookup"><span data-stu-id="7c308-302">For a conversion from `float` or `double` to `decimal`, the source value is converted to `decimal` representation and rounded to the nearest number after the 28th decimal place if required ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="7c308-303">如果來源值太小而無法表示為`decimal`，結果就會變成零。</span><span class="sxs-lookup"><span data-stu-id="7c308-303">If the source value is too small to represent as a `decimal`, the result becomes zero.</span></span> <span data-ttu-id="7c308-304">如果來源值是 NaN、 無限，或太大而無法表示為`decimal`、`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="7c308-304">If the source value is NaN, infinity, or too large to represent as a `decimal`, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="7c308-305">從轉換`decimal`要`float`或`double`，則`decimal`值會四捨五入為最接近`double`或`float`值。</span><span class="sxs-lookup"><span data-stu-id="7c308-305">For a conversion from `decimal` to `float` or `double`, the `decimal` value is rounded to the nearest `double` or `float` value.</span></span> <span data-ttu-id="7c308-306">雖然這種轉換可能會遺失有效位數，絕不會造成擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7c308-306">While this conversion may lose precision, it never causes an exception to be thrown.</span></span>

### <a name="explicit-enumeration-conversions"></a><span data-ttu-id="7c308-307">明確的列舉型別轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-307">Explicit enumeration conversions</span></span>

<span data-ttu-id="7c308-308">明確的列舉型別轉換為：</span><span class="sxs-lookup"><span data-stu-id="7c308-308">The explicit enumeration conversions are:</span></span>

*  <span data-ttu-id="7c308-309">從`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`decimal`至任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-309">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal` to any *enum_type*.</span></span>
*  <span data-ttu-id="7c308-310">從任何*enum_type*要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="7c308-310">From any *enum_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="7c308-311">從任何*enum_type*任何其他*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-311">From any *enum_type* to any other *enum_type*.</span></span>

<span data-ttu-id="7c308-312">兩個類型之間的明確的列舉型別轉換處理，是將任何參與*enum_type*為基礎的類型*enum_type*，然後執行隱含或明確產生的型別之間的數字轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-312">An explicit enumeration conversion between two types is processed by treating any participating *enum_type* as the underlying type of that *enum_type*, and then performing an implicit or explicit numeric conversion between the resulting types.</span></span> <span data-ttu-id="7c308-313">例如，假設*enum_type* `E`使用和的基礎類型`int`，從轉換`E`來`byte`處理成明確數值轉換 ([明確數值轉換](conversions.md#explicit-numeric-conversions)) 從`int`要`byte`，和從轉換`byte`來`E`處理時，隱含數值轉換 ([隱含數值轉換](conversions.md#implicit-numeric-conversions))從`byte`至`int`。</span><span class="sxs-lookup"><span data-stu-id="7c308-313">For example, given an *enum_type* `E` with and underlying type of `int`, a conversion from `E` to `byte` is processed as an explicit numeric conversion ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from `int` to `byte`, and a conversion from `byte` to `E` is processed as an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) from `byte` to `int`.</span></span>

### <a name="explicit-nullable-conversions"></a><span data-ttu-id="7c308-314">可為 null 的明確轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-314">Explicit nullable conversions</span></span>

<span data-ttu-id="7c308-315">***可為 null 的明確轉換***允許預先定義作用於不可為 null 的實值型別也可搭配可為 null 的表單，這些類型的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-315">***Explicit nullable conversions*** permit predefined explicit conversions that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="7c308-316">針對每個預先定義的明確轉換，從非可為 null 的實值型別轉換`S`不可為 null 的實值型別的`T`([識別轉換](conversions.md#identity-conversion)，[隱含數值轉換](conversions.md#implicit-numeric-conversions)，[隱含的列舉型別轉換](conversions.md#implicit-enumeration-conversions)，[明確數值轉換](conversions.md#explicit-numeric-conversions)，和[明確的列舉型別轉換](conversions.md#explicit-enumeration-conversions))，下列可為 null 的轉換存在：</span><span class="sxs-lookup"><span data-stu-id="7c308-316">For each of the predefined explicit conversions that convert from a non-nullable value type `S` to a non-nullable value type `T` ([Identity conversion](conversions.md#identity-conversion), [Implicit numeric conversions](conversions.md#implicit-numeric-conversions), [Implicit enumeration conversions](conversions.md#implicit-enumeration-conversions), [Explicit numeric conversions](conversions.md#explicit-numeric-conversions), and [Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)), the following nullable conversions exist:</span></span>

*  <span data-ttu-id="7c308-317">明確轉換`S?`至`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-317">An explicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="7c308-318">明確轉換`S`至`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-318">An explicit conversion from `S` to `T?`.</span></span>
*  <span data-ttu-id="7c308-319">明確轉換`S?`至`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-319">An explicit conversion from `S?` to `T`.</span></span>

<span data-ttu-id="7c308-320">可為 null 的轉換的評估會根據基礎的轉換，從`S`至`T`程序如下：</span><span class="sxs-lookup"><span data-stu-id="7c308-320">Evaluation of a nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="7c308-321">如果可為 null 的轉換是來自`S?`至`T?`:</span><span class="sxs-lookup"><span data-stu-id="7c308-321">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="7c308-322">如果來源值為 null (`HasValue`屬性為 false)，結果就是 null 值的型別`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-322">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="7c308-323">否則，則轉換會評估為從`S?`來`S`，後面接著從基礎轉換`S`來`T`，後面接著從繞`T`來`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-323">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="7c308-324">如果可為 null 的轉換是來自`S`來`T?`，則轉換會評估為基礎的轉換，從`S`來`T`後面接著從繞`T`來`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-324">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="7c308-325">如果可為 null 的轉換是來自`S?`來`T`，則轉換會評估為從`S?`來`S`後面接著從基礎轉換`S`至`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-325">If the nullable conversion is from `S?` to `T`, the conversion is evaluated as an unwrapping from `S?` to `S` followed by the underlying conversion from `S` to `T`.</span></span>

<span data-ttu-id="7c308-326">請注意，嘗試解除包裝的可為 null 的值將會擲回例外狀況的值是否`null`。</span><span class="sxs-lookup"><span data-stu-id="7c308-326">Note that an attempt to unwrap a nullable value will throw an exception if the value is `null`.</span></span>

### <a name="explicit-reference-conversions"></a><span data-ttu-id="7c308-327">明確參考轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-327">Explicit reference conversions</span></span>

<span data-ttu-id="7c308-328">明確參考轉換為：</span><span class="sxs-lookup"><span data-stu-id="7c308-328">The explicit reference conversions are:</span></span>

*  <span data-ttu-id="7c308-329">從`object`並`dynamic`任何其他*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-329">From `object` and `dynamic` to any other *reference_type*.</span></span>
*  <span data-ttu-id="7c308-330">從任何*class_type* `S`任何*class_type* `T`提供`S`的基底類別的`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-330">From any *class_type* `S` to any *class_type* `T`, provided `S` is a base class of `T`.</span></span>
*  <span data-ttu-id="7c308-331">從任何*class_type* `S`任何*interface_type* `T`提供`S`未密封且未提供`S`不會實作`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-331">From any *class_type* `S` to any *interface_type* `T`, provided `S` is not sealed and provided `S` does not implement `T`.</span></span>
*  <span data-ttu-id="7c308-332">從任何*interface_type* `S`任何*class_type* `T`提供`T`未密封格式，或提供`T`實作`S`。</span><span class="sxs-lookup"><span data-stu-id="7c308-332">From any *interface_type* `S` to any *class_type* `T`, provided `T` is not sealed or provided `T` implements `S`.</span></span>
*  <span data-ttu-id="7c308-333">從任何*interface_type* `S`任何*interface_type* `T`提供`S`不衍生自`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-333">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is not derived from `T`.</span></span>
*  <span data-ttu-id="7c308-334">從*array_type* `S`項目類型`SE`來*array_type* `T`項目類型`TE`，前提是所有下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="7c308-334">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="7c308-335">`S` 和`T`的差別只在於項目型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-335">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="7c308-336">亦即`S`和`T`有相同的維度數目。</span><span class="sxs-lookup"><span data-stu-id="7c308-336">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="7c308-337">兩者`SE`並`TE`會*reference_type*s。</span><span class="sxs-lookup"><span data-stu-id="7c308-337">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="7c308-338">明確參考轉換存在`SE`至`TE`。</span><span class="sxs-lookup"><span data-stu-id="7c308-338">An explicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="7c308-339">從`System.Array`介面的方式實作任何*array_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-339">From `System.Array` and the interfaces it implements to any *array_type*.</span></span>
*  <span data-ttu-id="7c308-340">從一維陣列的型別`S[]`要`System.Collections.Generic.IList<T>`和其基底介面，提供從明確參考轉換`S`至`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-340">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an explicit reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="7c308-341">從`System.Collections.Generic.IList<S>`和其基底介面為單一維度陣列型別`T[]`，前提是沒有明確的身分識別或參考轉換從`S`至`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-341">From `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]`, provided that there is an explicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="7c308-342">從`System.Delegate`介面的方式實作任何*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-342">From `System.Delegate` and the interfaces it implements to any *delegate_type*.</span></span>
*  <span data-ttu-id="7c308-343">從參考類型的參考型別`T`是否有參考類型的明確參考轉換`T0`並`T0`具有身分識別轉換`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-343">From a reference type to a reference type `T` if it has an explicit reference conversion to a reference type `T0` and `T0` has an identity conversion `T`.</span></span>
*  <span data-ttu-id="7c308-344">從介面或委派型別的參考型別`T`有明確參考轉換為介面或委派的型別`T0`任一個`T0`可轉換變異數來`T`或`T`是變異數可轉換為`T0`([變異數轉換](interfaces.md#variance-conversion))。</span><span class="sxs-lookup"><span data-stu-id="7c308-344">From a reference type to an interface or delegate type `T` if it has an explicit reference conversion to an interface or delegate type `T0` and either `T0` is variance-convertible to `T` or `T` is variance-convertible to `T0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>
*  <span data-ttu-id="7c308-345">從`D<S1...Sn>`來`D<T1...Tn>`何處`D<X1...Xn>`是泛型委派類型時，`D<S1...Sn>`不相容於或等於`D<T1...Tn>`，和每個類型參數`Xi`的`D`下列保留：</span><span class="sxs-lookup"><span data-stu-id="7c308-345">From `D<S1...Sn>` to `D<T1...Tn>` where `D<X1...Xn>` is a generic delegate type, `D<S1...Sn>` is not compatible with or identical to `D<T1...Tn>`, and for each type parameter `Xi` of `D` the following holds:</span></span>
    * <span data-ttu-id="7c308-346">如果`Xi`為非變異，然後`Si`等同於`Ti`。</span><span class="sxs-lookup"><span data-stu-id="7c308-346">If `Xi` is invariant, then `Si` is identical to `Ti`.</span></span>
    * <span data-ttu-id="7c308-347">如果`Xi`是 covariant，則是隱含或明確識別或參考轉換從`Si`至`Ti`。</span><span class="sxs-lookup"><span data-stu-id="7c308-347">If `Xi` is covariant, then there is an implicit or explicit identity or reference conversion from `Si` to `Ti`.</span></span>
    * <span data-ttu-id="7c308-348">如果`Xi`是逆變性，則`Si`和`Ti`都是相同或是兩者都參考型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-348">If `Xi` is contravariant, then `Si` and `Ti` are either identical or both reference types.</span></span>
*  <span data-ttu-id="7c308-349">涉及為參考型別的已知型別參數的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-349">Explicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="7c308-350">如需涉及型別參數的明確轉換的詳細資訊，請參閱 <<c0> [ 涉及型別參數的明確轉換](conversions.md#explicit-conversions-involving-type-parameters)。</span><span class="sxs-lookup"><span data-stu-id="7c308-350">For more details on explicit conversions involving type parameters, see [Explicit conversions involving type parameters](conversions.md#explicit-conversions-involving-type-parameters).</span></span>

<span data-ttu-id="7c308-351">明確參考轉換，是指需要執行階段檢查，以確保其為正確的參考類型之間轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-351">The explicit reference conversions are those conversions between reference-types that require run-time checks to ensure they are correct.</span></span>

<span data-ttu-id="7c308-352">若要在執行階段成功明確參考轉換，來源運算元的值必須是`null`，或來源運算元所參考的物件的實際型別必須是可以轉換成目的地類型的隱含參考的類型轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 或 boxing 轉換 ([Boxing 轉換](conversions.md#boxing-conversions))。</span><span class="sxs-lookup"><span data-stu-id="7c308-352">For an explicit reference conversion to succeed at run-time, the value of the source operand must be `null`, or the actual type of the object referenced by the source operand must be a type that can be converted to the destination type by an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)).</span></span> <span data-ttu-id="7c308-353">如果明確參考轉換失敗，`System.InvalidCastException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="7c308-353">If an explicit reference conversion fails, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="7c308-354">參考轉換，隱含或明確的永遠不會變更轉換的物件參考的識別。</span><span class="sxs-lookup"><span data-stu-id="7c308-354">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="7c308-355">換句話說，雖然參考轉換可能會變更參考的類型，但它絕不會變更型別或所參考之物件的值。</span><span class="sxs-lookup"><span data-stu-id="7c308-355">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="7c308-356">Unboxing 轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-356">Unboxing conversions</span></span>

<span data-ttu-id="7c308-357">Unboxing 轉換允許明確轉換成參考型別*value_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-357">An unboxing conversion permits a reference type to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="7c308-358">Unboxing 轉換存在從型別`object`，`dynamic`並`System.ValueType`任何*non_nullable_value_type*，並從任何*interface_type*任何*non_nullable_value_type*可實*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-358">An unboxing conversion exists from the types `object`, `dynamic` and `System.ValueType` to any *non_nullable_value_type*, and from any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span> <span data-ttu-id="7c308-359">此外鍵入`System.Enum`可以是任何 unboxed *enum_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-359">Furthermore type `System.Enum` can be unboxed to any *enum_type*.</span></span>

<span data-ttu-id="7c308-360">Unboxing 轉換存在從參考的型別*nullable_type*有 unboxing 轉換的參考型別從基礎*non_nullable_value_type*的*nullable_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-360">An unboxing conversion exists from a reference type to a *nullable_type* if an unboxing conversion exists from the reference type to the underlying *non_nullable_value_type* of the *nullable_type*.</span></span>

<span data-ttu-id="7c308-361">實值型別`S`具有 unboxing 轉換將介面類型`I`是否有從介面類型 unboxing 轉換`I0`並`I0`具有身分識別轉換至`I`。</span><span class="sxs-lookup"><span data-stu-id="7c308-361">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="7c308-362">實值型別`S`已從介面類型 unboxing 轉換`I`有 unboxing 轉換從介面或委派的型別`I0`任一個`I0`可轉換變異數來`I`或`I`可轉換變異數來`I0`([變異數轉換](interfaces.md#variance-conversion))。</span><span class="sxs-lookup"><span data-stu-id="7c308-362">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface or delegate type `I0` and either `I0` is variance-convertible to `I` or `I` is variance-convertible to `I0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>

<span data-ttu-id="7c308-363">Unboxing 作業包含第一個檢查的物件執行個體的 boxed 的值的指定*value_type*，然後複製到執行個體的值。</span><span class="sxs-lookup"><span data-stu-id="7c308-363">An unboxing operation consists of first checking that the object instance is a boxed value of the given *value_type*, and then copying the value out of the instance.</span></span> <span data-ttu-id="7c308-364">Unboxing 的 null 參考*nullable_type*產生 null 值的*nullable_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-364">Unboxing a null reference to a *nullable_type* produces the null value of the *nullable_type*.</span></span> <span data-ttu-id="7c308-365">結構可以從型別 unboxed `System.ValueType`，因為這是全部為結構的基底類別 ([繼承](structs.md#inheritance))。</span><span class="sxs-lookup"><span data-stu-id="7c308-365">A struct can be unboxed from the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="7c308-366">Unboxing 轉換中所述進一步[Unboxing 轉換](types.md#unboxing-conversions)。</span><span class="sxs-lookup"><span data-stu-id="7c308-366">Unboxing conversions are described further in [Unboxing conversions](types.md#unboxing-conversions).</span></span>

### <a name="explicit-dynamic-conversions"></a><span data-ttu-id="7c308-367">明確的動態轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-367">Explicit dynamic conversions</span></span>

<span data-ttu-id="7c308-368">從類型的運算式存在明確的動態轉換`dynamic`任何型別的`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-368">An explicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="7c308-369">轉換動態繫結 ([動態繫結](expressions.md#dynamic-binding))，這表示，將會在執行階段從執行階段類型的運算式來搜尋的明確轉換`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-369">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an explicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="7c308-370">如果不找到任何轉換，則會擲回執行階段例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7c308-370">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="7c308-371">如果不需要轉換的動態繫結，運算式可以先轉換成`object`，然後將所需的類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-371">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="7c308-372">假設下列類別的定義：</span><span class="sxs-lookup"><span data-stu-id="7c308-372">Assume the following class is defined:</span></span>
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

<span data-ttu-id="7c308-373">下列範例說明明確的動態轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-373">The following example illustrates explicit dynamic conversions:</span></span>
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

<span data-ttu-id="7c308-374">最佳轉換`o`至`C`找到在編譯時期要明確參考轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-374">The best conversion of `o` to `C` is found at compile-time to be an explicit reference conversion.</span></span> <span data-ttu-id="7c308-375">這會失敗，在執行階段，因為`"1"`不是事實`C`。</span><span class="sxs-lookup"><span data-stu-id="7c308-375">This fails at run-time, because `"1"` is not in fact a `C`.</span></span> <span data-ttu-id="7c308-376">轉換`d`要`C`不過，做為明確的動態轉換，則會暫停執行階段，其中使用者定義的執行階段類型的轉換`d`  --  `string` -為`C`找到，便會成功。</span><span class="sxs-lookup"><span data-stu-id="7c308-376">The conversion of `d` to `C` however, as an explicit dynamic conversion, is suspended to run-time, where a user defined conversion from the run-time type of `d` -- `string` -- to `C` is found, and succeeds.</span></span>

### <a name="explicit-conversions-involving-type-parameters"></a><span data-ttu-id="7c308-377">包含型別參數的明確轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-377">Explicit conversions involving type parameters</span></span>

<span data-ttu-id="7c308-378">下列的明確轉換存在指定的型別參數`T`:</span><span class="sxs-lookup"><span data-stu-id="7c308-378">The following explicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="7c308-379">從有效的基底類別`C`的`T`要`T`並從任何基底類別`C`到`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-379">From the effective base class `C` of `T` to `T` and from any base class of `C` to `T`.</span></span> <span data-ttu-id="7c308-380">在執行階段，如果`T`是實值類型，轉換會執行為 unboxing 轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-380">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="7c308-381">否則，轉換會執行為明確參考轉換或識別轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-381">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="7c308-382">從任何介面類型到`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-382">From any interface type to `T`.</span></span> <span data-ttu-id="7c308-383">在執行階段，如果`T`是實值類型，轉換會執行為 unboxing 轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-383">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="7c308-384">否則，轉換會執行為明確參考轉換或識別轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-384">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="7c308-385">從`T`任何*interface_type* `I`提供的還沒有的隱含轉換`T`至`I`。</span><span class="sxs-lookup"><span data-stu-id="7c308-385">From `T` to any *interface_type* `I` provided there is not already an implicit conversion from `T` to `I`.</span></span> <span data-ttu-id="7c308-386">在執行階段，如果`T`是實值類型，轉換會執行為 boxing 轉換，後面接著明確參考轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-386">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion followed by an explicit reference conversion.</span></span> <span data-ttu-id="7c308-387">否則，轉換會執行為明確參考轉換或識別轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-387">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="7c308-388">從型別參數`U`要`T`提供`T`取決於`U`([類型參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="7c308-388">From a type parameter `U` to `T`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="7c308-389">在執行階段，如果`U`是實值類型，則`T`和`U`一定都是相同的型別並不會執行轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-389">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="7c308-390">否則，如果`T`是實值類型，轉換會執行為 unboxing 轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-390">Otherwise, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="7c308-391">否則，轉換會執行為明確參考轉換或識別轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-391">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="7c308-392">如果`T`是已知為參考型別，則上述轉換全都歸類為明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="7c308-392">If `T` is known to be a reference type, the conversions above are all classified as explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="7c308-393">如果`T`不是知道是參考類型，上述的轉換會歸類為 unboxing 轉換 ([Unboxing 轉換](conversions.md#unboxing-conversions))。</span><span class="sxs-lookup"><span data-stu-id="7c308-393">If `T` is not known to be a reference type, the conversions above are classified as unboxing conversions ([Unboxing conversions](conversions.md#unboxing-conversions)).</span></span>

<span data-ttu-id="7c308-394">上述規則不允許直接從明確轉換的未受限制的型別參數至非介面的類型，這可能會令人意外。</span><span class="sxs-lookup"><span data-stu-id="7c308-394">The above rules do not permit a direct explicit conversion from an unconstrained type parameter to a non-interface type, which might be surprising.</span></span> <span data-ttu-id="7c308-395">此規則的原因是避免混淆，這類清除的轉換語意。</span><span class="sxs-lookup"><span data-stu-id="7c308-395">The reason for this rule is to prevent confusion and make the semantics of such conversions clear.</span></span> <span data-ttu-id="7c308-396">例如，請參考下列宣告：</span><span class="sxs-lookup"><span data-stu-id="7c308-396">For example, consider the following declaration:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

<span data-ttu-id="7c308-397">如果直接明確轉換`t`要`int`允許的可能會輕易地以為所`X<int>.F(7)`會傳回 `7L`。</span><span class="sxs-lookup"><span data-stu-id="7c308-397">If the direct explicit conversion of `t` to `int` were permitted, one might easily expect that `X<int>.F(7)` would return `7L`.</span></span> <span data-ttu-id="7c308-398">不過，它不會，因為已知型別是數值繫結時間時，只會考慮標準的數字轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-398">However, it would not, because the standard numeric conversions are only considered when the types are known to be numeric at binding-time.</span></span> <span data-ttu-id="7c308-399">若要進行語意清楚、 上述的範例必須改為撰寫：</span><span class="sxs-lookup"><span data-stu-id="7c308-399">In order to make the semantics clear, the above example must instead be written:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

<span data-ttu-id="7c308-400">此程式碼現在會進行編譯但執行`X<int>.F(7)`會再擲回例外狀況在執行階段自 boxed`int`無法直接轉換成`long`。</span><span class="sxs-lookup"><span data-stu-id="7c308-400">This code will now compile but executing `X<int>.F(7)` would then throw an exception at run-time, since a boxed `int` cannot be converted directly to a `long`.</span></span>

### <a name="user-defined-explicit-conversions"></a><span data-ttu-id="7c308-401">使用者定義的明確轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-401">User-defined explicit conversions</span></span>

<span data-ttu-id="7c308-402">使用者定義的明確轉換所組成的選擇性標準明確轉換，後面接著執行使用者定義的隱含或明確轉換運算子，後面接著另一個選用標準的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-402">A user-defined explicit conversion consists of an optional standard explicit conversion, followed by execution of a user-defined implicit or explicit conversion operator, followed by another optional standard explicit conversion.</span></span> <span data-ttu-id="7c308-403">評估使用者定義的明確轉換的確切規則所述[處理的使用者定義的明確轉換](conversions.md#processing-of-user-defined-explicit-conversions)。</span><span class="sxs-lookup"><span data-stu-id="7c308-403">The exact rules for evaluating user-defined explicit conversions are described in [Processing of user-defined explicit conversions](conversions.md#processing-of-user-defined-explicit-conversions).</span></span>

## <a name="standard-conversions"></a><span data-ttu-id="7c308-404">標準轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-404">Standard conversions</span></span>

<span data-ttu-id="7c308-405">標準的轉換是轉換的預先定義的使用者定義過程中可能發生的。</span><span class="sxs-lookup"><span data-stu-id="7c308-405">The standard conversions are those pre-defined conversions that can occur as part of a user-defined conversion.</span></span>

### <a name="standard-implicit-conversions"></a><span data-ttu-id="7c308-406">標準的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-406">Standard implicit conversions</span></span>

<span data-ttu-id="7c308-407">下列的隱含轉換會歸類為標準的隱含轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-407">The following implicit conversions are classified as standard implicit conversions:</span></span>

*  <span data-ttu-id="7c308-408">身分識別轉換 ([身分識別轉換](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="7c308-408">Identity conversions ([Identity conversion](conversions.md#identity-conversion))</span></span>
*  <span data-ttu-id="7c308-409">隱含數值轉換 ([隱含數值轉換](conversions.md#implicit-numeric-conversions))</span><span class="sxs-lookup"><span data-stu-id="7c308-409">Implicit numeric conversions ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions))</span></span>
*  <span data-ttu-id="7c308-410">可為 null 的隱含轉換 ([可為 null 的隱含轉換](conversions.md#implicit-nullable-conversions))</span><span class="sxs-lookup"><span data-stu-id="7c308-410">Implicit nullable conversions ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions))</span></span>
*  <span data-ttu-id="7c308-411">隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="7c308-411">Implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
*  <span data-ttu-id="7c308-412">Boxing 轉換 ([Boxing 轉換](conversions.md#boxing-conversions))</span><span class="sxs-lookup"><span data-stu-id="7c308-412">Boxing conversions ([Boxing conversions](conversions.md#boxing-conversions))</span></span>
*  <span data-ttu-id="7c308-413">隱含的常數運算式轉換 ([隱含的動態轉換](conversions.md#implicit-dynamic-conversions))</span><span class="sxs-lookup"><span data-stu-id="7c308-413">Implicit constant expression conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions))</span></span>
*  <span data-ttu-id="7c308-414">包含型別參數的隱含轉換 ([涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters))</span><span class="sxs-lookup"><span data-stu-id="7c308-414">Implicit conversions involving type parameters ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters))</span></span>

<span data-ttu-id="7c308-415">標準的隱含轉換特別排除使用者定義的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-415">The standard implicit conversions specifically exclude user-defined implicit conversions.</span></span>

### <a name="standard-explicit-conversions"></a><span data-ttu-id="7c308-416">標準的明確轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-416">Standard explicit conversions</span></span>

<span data-ttu-id="7c308-417">標準的明確轉換，是所有標準的隱含轉換，再加上存在的相反的標準的隱含轉換的明確轉換的子集。</span><span class="sxs-lookup"><span data-stu-id="7c308-417">The standard explicit conversions are all standard implicit conversions plus the subset of the explicit conversions for which an opposite standard implicit conversion exists.</span></span> <span data-ttu-id="7c308-418">換句話說，如果隱含的標準轉換存在從型別`A`型別`B`，則標準的明確轉換存在從型別`A`鍵入`B`與從型別`B`輸入`A`。</span><span class="sxs-lookup"><span data-stu-id="7c308-418">In other words, if a standard implicit conversion exists from a type `A` to a type `B`, then a standard explicit conversion exists from type `A` to type `B` and from type `B` to type `A`.</span></span>

## <a name="user-defined-conversions"></a><span data-ttu-id="7c308-419">使用者定義轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-419">User-defined conversions</span></span>

<span data-ttu-id="7c308-420">C# 允許預先定義的隱含和明確轉換成增強***使用者定義轉換***。</span><span class="sxs-lookup"><span data-stu-id="7c308-420">C# allows the pre-defined implicit and explicit conversions to be augmented by ***user-defined conversions***.</span></span> <span data-ttu-id="7c308-421">使用者定義轉換導入的宣告轉換運算子 ([轉換運算子](classes.md#conversion-operators)) 類別和結構的類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-421">User-defined conversions are introduced by declaring conversion operators ([Conversion operators](classes.md#conversion-operators)) in class and struct types.</span></span>

### <a name="permitted-user-defined-conversions"></a><span data-ttu-id="7c308-422">允許使用者定義轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-422">Permitted user-defined conversions</span></span>

<span data-ttu-id="7c308-423">C# 允許只有特定使用者定義轉換為宣告。</span><span class="sxs-lookup"><span data-stu-id="7c308-423">C# permits only certain user-defined conversions to be declared.</span></span> <span data-ttu-id="7c308-424">特別是，不能重新定義現有的隱含或明確轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-424">In particular, it is not possible to redefine an already existing implicit or explicit conversion.</span></span>

<span data-ttu-id="7c308-425">指定的來源型別的`S`和目標型別`T`，如果`S`或`T`是可為 null 的型別，讓`S0`並`T0`指其基礎類型，否則為`S0`和`T0`是相等`S`和`T`分別。</span><span class="sxs-lookup"><span data-stu-id="7c308-425">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="7c308-426">類別或結構，並允許宣告從來源類型轉換`S`成目標型別`T`只有當下列所有項目為真：</span><span class="sxs-lookup"><span data-stu-id="7c308-426">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="7c308-427">`S0` 和`T0`是不同的型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-427">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="7c308-428">請`S0`或`T0`是運算子宣告進行的類別或結構類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-428">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="7c308-429">既不`S0`也`T0`是*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="7c308-429">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="7c308-430">排除使用者定義的轉換，轉換沒有從`S`來`T`或來自`T`至`S`。</span><span class="sxs-lookup"><span data-stu-id="7c308-430">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="7c308-431">以使用者定義轉換適用之限制的討論中進一步[轉換運算子](classes.md#conversion-operators)。</span><span class="sxs-lookup"><span data-stu-id="7c308-431">The restrictions that apply to user-defined conversions are discussed further in [Conversion operators](classes.md#conversion-operators).</span></span>

### <a name="lifted-conversion-operators"></a><span data-ttu-id="7c308-432">提昇的轉換運算子</span><span class="sxs-lookup"><span data-stu-id="7c308-432">Lifted conversion operators</span></span>

<span data-ttu-id="7c308-433">指定使用者定義轉換運算子可從不可為 null 的實值型別轉換`S`不可為 null 的實值型別的`T`，則***提昇的轉換運算子***存在從轉換`S?`到`T?`.</span><span class="sxs-lookup"><span data-stu-id="7c308-433">Given a user-defined conversion operator that converts from a non-nullable value type `S` to a non-nullable value type `T`, a ***lifted conversion operator*** exists that converts from `S?` to `T?`.</span></span> <span data-ttu-id="7c308-434">此提昇的轉換運算子會執行從形式`S?`來`S`後面接著從使用者定義轉換`S`來`T`後面接著從繞`T`至`T?`，不同之處在於 nullvalued`S?`將轉換成 null 值直接`T?`。</span><span class="sxs-lookup"><span data-stu-id="7c308-434">This lifted conversion operator performs an unwrapping from `S?` to `S` followed by the user-defined conversion from `S` to `T` followed by a wrapping from `T` to `T?`, except that a null valued `S?` converts directly to a null valued `T?`.</span></span>

<span data-ttu-id="7c308-435">提昇的轉換運算子有相同的隱含或明確分類，作為其基礎的使用者定義轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-435">A lifted conversion operator has the same implicit or explicit classification as its underlying user-defined conversion operator.</span></span> <span data-ttu-id="7c308-436">使用適用於 「 使用者定義轉換 」 一詞使用者定義，並提昇的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-436">The term "user-defined conversion" applies to the use of both user-defined and lifted conversion operators.</span></span>

### <a name="evaluation-of-user-defined-conversions"></a><span data-ttu-id="7c308-437">使用者定義轉換的評估結果</span><span class="sxs-lookup"><span data-stu-id="7c308-437">Evaluation of user-defined conversions</span></span>

<span data-ttu-id="7c308-438">使用者定義的轉換會將值轉換自其型別，稱為***來源類型***，另一種類型，稱為***目標類型***。</span><span class="sxs-lookup"><span data-stu-id="7c308-438">A user-defined conversion converts a value from its type, called the ***source type***, to another type, called the ***target type***.</span></span> <span data-ttu-id="7c308-439">評估使用者定義轉換的中心有關尋找***最特定***尋找特定的來源和目標類型的使用者定義轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-439">Evaluation of a user-defined conversion centers on finding the ***most specific*** user-defined conversion operator for the particular source and target types.</span></span> <span data-ttu-id="7c308-440">這項判斷會分成幾個步驟：</span><span class="sxs-lookup"><span data-stu-id="7c308-440">This determination is broken into several steps:</span></span>

*  <span data-ttu-id="7c308-441">尋找一組類別和結構會被視為使用者定義轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-441">Finding the set of classes and structs from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="7c308-442">這個集合是由來源類型和其基底類別和目標型別和其基底類別 （只有類別和結構可以宣告使用者定義的運算子和非類別類型具有基底類別的隱含假設） 所組成。</span><span class="sxs-lookup"><span data-stu-id="7c308-442">This set consists of the source type and its base classes and the target type and its base classes (with the implicit assumptions that only classes and structs can declare user-defined operators, and that non-class types have no base classes).</span></span> <span data-ttu-id="7c308-443">此步驟中，如果來源或目標類型是基於*nullable_type*、 其基礎類型改為使用。</span><span class="sxs-lookup"><span data-stu-id="7c308-443">For the purposes of this step, if either the source or target type is a *nullable_type*, their underlying type is used instead.</span></span>
*  <span data-ttu-id="7c308-444">從該型別組合，決定其使用者定義和提昇的轉換運算子皆適用。</span><span class="sxs-lookup"><span data-stu-id="7c308-444">From that set of types, determining which user-defined and lifted conversion operators are applicable.</span></span> <span data-ttu-id="7c308-445">適用於轉換運算子，它必須能夠執行標準轉換 ([標準轉換](conversions.md#standard-conversions)) 從來源類型運算元的運算子和它的型別必須是能夠執行標準轉換從目標型別運算子的結果類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-445">For a conversion operator to be applicable, it must be possible to perform a standard conversion ([Standard conversions](conversions.md#standard-conversions)) from the source type to the operand type of the operator, and it must be possible to perform a standard conversion from the result type of the operator to the target type.</span></span>
*  <span data-ttu-id="7c308-446">從適用於使用者定義的運算子，判斷哪個運算子最適合明確地設定。</span><span class="sxs-lookup"><span data-stu-id="7c308-446">From the set of applicable user-defined operators, determining which operator is unambiguously the most specific.</span></span> <span data-ttu-id="7c308-447">一般而言，最特定的運算子會是其運算元的類型是 「 最靠近 」 的來源類型，且結果類型為 「 最靠近 」 的目標類型的運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-447">In general terms, the most specific operator is the operator whose operand type is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="7c308-448">使用者定義轉換運算子是慣用的透過消除的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-448">User-defined conversion operators are preferred over lifted conversion operators.</span></span> <span data-ttu-id="7c308-449">下列各節中定義建立最明確的使用者定義轉換運算子的確切規則。</span><span class="sxs-lookup"><span data-stu-id="7c308-449">The exact rules for establishing the most specific user-defined conversion operator are defined in the following sections.</span></span>

<span data-ttu-id="7c308-450">一旦已識別出最明確的使用者定義轉換運算子，實際執行使用者定義的轉換就會包含最多三個步驟：</span><span class="sxs-lookup"><span data-stu-id="7c308-450">Once a most specific user-defined conversion operator has been identified, the actual execution of the user-defined conversion involves up to three steps:</span></span>

*  <span data-ttu-id="7c308-451">首先，若有必要，執行從來源類型的標準轉換成使用者定義或提昇轉換運算子的運算元型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-451">First, if required, performing a standard conversion from the source type to the operand type of the user-defined or lifted conversion operator.</span></span>
*  <span data-ttu-id="7c308-452">接下來，您可以叫用使用者定義或提昇轉換運算子，以執行轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-452">Next, invoking the user-defined or lifted conversion operator to perform the conversion.</span></span>
*  <span data-ttu-id="7c308-453">最後，若有必要，執行從使用者定義或提昇轉換運算子的結果類型的標準轉換成目標型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-453">Finally, if required, performing a standard conversion from the result type of the user-defined or lifted conversion operator to the target type.</span></span>

<span data-ttu-id="7c308-454">使用者定義轉換永遠不會評估涉及一個以上的使用者定義或提昇轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-454">Evaluation of a user-defined conversion never involves more than one user-defined or lifted conversion operator.</span></span> <span data-ttu-id="7c308-455">換句話說，從類型轉換`S`輸入`T`將永遠不會先執行使用者定義的轉換，從`S`要`X`，然後執行使用者定義的轉換，從`X`來`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-455">In other words, a conversion from type `S` to type `T` will never first execute a user-defined conversion from `S` to `X` and then execute a user-defined conversion from `X` to `T`.</span></span>

<span data-ttu-id="7c308-456">下列各節會提供評估的使用者定義的隱含或明確轉換的確切定義。</span><span class="sxs-lookup"><span data-stu-id="7c308-456">Exact definitions of evaluation of user-defined implicit or explicit conversions are given in the following sections.</span></span> <span data-ttu-id="7c308-457">定義將使用下列詞彙：</span><span class="sxs-lookup"><span data-stu-id="7c308-457">The definitions make use of the following terms:</span></span>

*  <span data-ttu-id="7c308-458">如果是標準的隱含轉換 ([標準的隱含轉換](conversions.md#standard-implicit-conversions)) 存在從型別`A`型別`B`，和如果既未`A`也`B`是*interface_type*s，然後`A`要***包含*** `B`，和`B`稱為***包含*** `A`。</span><span class="sxs-lookup"><span data-stu-id="7c308-458">If a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) exists from a type `A` to a type `B`, and if neither `A` nor `B` are *interface_type*s, then `A` is said to be ***encompassed by*** `B`, and `B` is said to ***encompass*** `A`.</span></span>
*  <span data-ttu-id="7c308-459">***最內含的型別***中一組型別是一個包含集合中的所有其他類型的類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-459">The ***most encompassing type*** in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="7c308-460">如果沒有單一的型別包含所有其他型別，集合都沒有最內含的型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-460">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="7c308-461">更具直覺性而言，最內含的型別是集合中的 「 大 」 類型，每個其他類型可以隱含地轉換為一種類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-461">In more intuitive terms, the most encompassing type is the "largest" type in the set—the one type to which each of the other types can be implicitly converted.</span></span>
*  <span data-ttu-id="7c308-462">***最所包含型別***中一組型別是集合中的所有型別包含一種類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-462">The ***most encompassed type*** in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="7c308-463">如果沒有單一的型別被包含所有其他型別時，集合已最不會包含型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-463">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="7c308-464">更具直覺性而言，最被內含的類型是 「 小 」 的型別集合中，能夠隱含轉換成其他類型的每一種類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-464">In more intuitive terms, the most encompassed type is the "smallest" type in the set—the one type that can be implicitly converted to each of the other types.</span></span>

### <a name="processing-of-user-defined-implicit-conversions"></a><span data-ttu-id="7c308-465">處理的使用者定義的隱含轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-465">Processing of user-defined implicit conversions</span></span>

<span data-ttu-id="7c308-466">從型別，使用者定義的隱含轉換`S`輸入`T`處理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7c308-466">A user-defined implicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="7c308-467">判斷型別`S0`和`T0`。</span><span class="sxs-lookup"><span data-stu-id="7c308-467">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="7c308-468">如果`S`或`T`是可為 null 的型別，`S0`並`T0`是其基礎類型，否則為`S0`並`T0`等於`S`和`T`分別。</span><span class="sxs-lookup"><span data-stu-id="7c308-468">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="7c308-469">尋找一組類型， `D`，運算子會被視為從哪一個使用者定義的轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-469">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="7c308-470">這組組成`S0`(如果`S0`是類別或結構)，基底類別的`S0`(如果`S0`是一種類別)，並`T0`(如果`T0`是類別或結構)。</span><span class="sxs-lookup"><span data-stu-id="7c308-470">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), and `T0` (if `T0` is a class or struct).</span></span>
*  <span data-ttu-id="7c308-471">尋找一組適用於使用者定義和提昇轉換運算子， `U`。</span><span class="sxs-lookup"><span data-stu-id="7c308-471">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="7c308-472">這個集合所組成的類別或結構中的所宣告的使用者定義和提昇的隱含轉換運算子`D`，將從型別，包含轉換`S`所包含的型別`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-472">This set consists of the user-defined and lifted implicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing `S` to a type encompassed by `T`.</span></span> <span data-ttu-id="7c308-473">如果`U`是空的轉換為未定義，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-473">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="7c308-474">尋找最特定的來源類型，`SX`中的運算子`U`:</span><span class="sxs-lookup"><span data-stu-id="7c308-474">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="7c308-475">如果任一項中的運算子`U`轉換從`S`，然後`SX`是`S`。</span><span class="sxs-lookup"><span data-stu-id="7c308-475">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="7c308-476">否則，請`SX`是來源型別之運算子的組合中最置於包含的型別`U`。</span><span class="sxs-lookup"><span data-stu-id="7c308-476">Otherwise, `SX` is the most encompassed type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="7c308-477">如果只有一個最包含找不到型別，則轉換模稜兩可和就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-477">If exactly one most encompassed type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="7c308-478">尋找最特定的目標型別，`TX`中的運算子`U`:</span><span class="sxs-lookup"><span data-stu-id="7c308-478">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="7c308-479">如果任一項中的運算子`U`轉換成`T`，然後`TX`是`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-479">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="7c308-480">否則，請`TX`是最內含的型別，在目標型別中的運算子組合`U`。</span><span class="sxs-lookup"><span data-stu-id="7c308-480">Otherwise, `TX` is the most encompassing type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="7c308-481">如果找不到一個最內含的型別，則轉換是模稜兩可，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-481">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="7c308-482">尋找最明確的轉換運算子：</span><span class="sxs-lookup"><span data-stu-id="7c308-482">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="7c308-483">如果`U`包含一個會將從轉換的使用者定義的轉換運算子`SX`到`TX`，則這是最特定的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-483">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="7c308-484">否則，如果`U`包含一個會將從轉換的提昇的轉換運算子`SX`到`TX`，則這是最特定的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-484">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="7c308-485">否則，轉換模稜兩可，而且就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-485">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="7c308-486">最後，將套用的轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-486">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="7c308-487">如果`S`不是`SX`，然後從標準隱含轉換`S`到`SX`會執行。</span><span class="sxs-lookup"><span data-stu-id="7c308-487">If `S` is not `SX`, then a standard implicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="7c308-488">最特定的轉換運算子會叫用來從轉換`SX`至`TX`。</span><span class="sxs-lookup"><span data-stu-id="7c308-488">The most specific conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="7c308-489">如果`TX`不是`T`，然後從標準隱含轉換`TX`到`T`會執行。</span><span class="sxs-lookup"><span data-stu-id="7c308-489">If `TX` is not `T`, then a standard implicit conversion from `TX` to `T` is performed.</span></span>

### <a name="processing-of-user-defined-explicit-conversions"></a><span data-ttu-id="7c308-490">處理的使用者定義的明確轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-490">Processing of user-defined explicit conversions</span></span>

<span data-ttu-id="7c308-491">從型別，使用者定義的明確轉換`S`輸入`T`處理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7c308-491">A user-defined explicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="7c308-492">判斷型別`S0`和`T0`。</span><span class="sxs-lookup"><span data-stu-id="7c308-492">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="7c308-493">如果`S`或`T`是可為 null 的型別，`S0`並`T0`是其基礎類型，否則為`S0`並`T0`等於`S`和`T`分別。</span><span class="sxs-lookup"><span data-stu-id="7c308-493">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="7c308-494">尋找一組類型， `D`，運算子會被視為從哪一個使用者定義的轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-494">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="7c308-495">這組組成`S0`(如果`S0`是類別或結構)，基底類別`S0`(如果`S0`是一種類別)， `T0` (如果`T0`是類別或結構)，和基底類別的`T0`(如果`T0`是類別)。</span><span class="sxs-lookup"><span data-stu-id="7c308-495">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), `T0` (if `T0` is a class or struct), and the base classes of `T0` (if `T0` is a class).</span></span>
*  <span data-ttu-id="7c308-496">尋找一組適用於使用者定義和提昇轉換運算子， `U`。</span><span class="sxs-lookup"><span data-stu-id="7c308-496">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="7c308-497">這一組含有使用者定義和提昇隱含或明確轉換運算子宣告的類別或結構中的所`D`，從內含型別轉換，或包含`S`至內含型別所`T`.</span><span class="sxs-lookup"><span data-stu-id="7c308-497">This set consists of the user-defined and lifted implicit or explicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing or encompassed by `S` to a type encompassing or encompassed by `T`.</span></span> <span data-ttu-id="7c308-498">如果`U`是空的轉換為未定義，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-498">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="7c308-499">尋找最特定的來源類型，`SX`中的運算子`U`:</span><span class="sxs-lookup"><span data-stu-id="7c308-499">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="7c308-500">如果任一項中的運算子`U`轉換從`S`，然後`SX`是`S`。</span><span class="sxs-lookup"><span data-stu-id="7c308-500">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="7c308-501">否則，如果任一項中的運算子`U`從包含的類型轉換`S`，然後`SX`是最置於包含的型別，這些運算子的來源類型的組合中。</span><span class="sxs-lookup"><span data-stu-id="7c308-501">Otherwise, if any of the operators in `U` convert from types that encompass `S`, then `SX` is the most encompassed type in the combined set of source types of those operators.</span></span> <span data-ttu-id="7c308-502">如果沒有大部分包含可以找到型別，則轉換模稜兩可和就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-502">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="7c308-503">否則，請`SX`是來源型別之運算子的組合中的最內含類型`U`。</span><span class="sxs-lookup"><span data-stu-id="7c308-503">Otherwise, `SX` is the most encompassing type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="7c308-504">如果找不到一個最內含的型別，則轉換是模稜兩可，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-504">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="7c308-505">尋找最特定的目標型別，`TX`中的運算子`U`:</span><span class="sxs-lookup"><span data-stu-id="7c308-505">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="7c308-506">如果任一項中的運算子`U`轉換成`T`，然後`TX`是`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-506">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="7c308-507">否則，如果任一項中的運算子`U`轉換為所包含的類型`T`，然後`TX`是最內含的型別，這些運算子的目標類型的組合中。</span><span class="sxs-lookup"><span data-stu-id="7c308-507">Otherwise, if any of the operators in `U` convert to types that are encompassed by `T`, then `TX` is the most encompassing type in the combined set of target types of those operators.</span></span> <span data-ttu-id="7c308-508">如果找不到一個最內含的型別，則轉換是模稜兩可，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-508">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="7c308-509">否則，請`TX`是最置於包含的型別中運算子的目標類型的組合中`U`。</span><span class="sxs-lookup"><span data-stu-id="7c308-509">Otherwise, `TX` is the most encompassed type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="7c308-510">如果沒有大部分包含可以找到型別，則轉換模稜兩可和就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-510">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="7c308-511">尋找最明確的轉換運算子：</span><span class="sxs-lookup"><span data-stu-id="7c308-511">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="7c308-512">如果`U`包含一個會將從轉換的使用者定義的轉換運算子`SX`到`TX`，則這是最特定的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-512">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="7c308-513">否則，如果`U`包含一個會將從轉換的提昇的轉換運算子`SX`到`TX`，則這是最特定的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="7c308-513">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="7c308-514">否則，轉換模稜兩可，而且就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-514">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="7c308-515">最後，將套用的轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-515">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="7c308-516">如果`S`不是`SX`，然後從標準明確轉換`S`到`SX`會執行。</span><span class="sxs-lookup"><span data-stu-id="7c308-516">If `S` is not `SX`, then a standard explicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="7c308-517">最特定的使用者定義轉換運算子會叫用來從轉換`SX`至`TX`。</span><span class="sxs-lookup"><span data-stu-id="7c308-517">The most specific user-defined conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="7c308-518">如果`TX`不是`T`，然後從標準明確轉換`TX`到`T`會執行。</span><span class="sxs-lookup"><span data-stu-id="7c308-518">If `TX` is not `T`, then a standard explicit conversion from `TX` to `T` is performed.</span></span>

## <a name="anonymous-function-conversions"></a><span data-ttu-id="7c308-519">匿名函式轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-519">Anonymous function conversions</span></span>

<span data-ttu-id="7c308-520">*Anonymous_method_expression*或是*lambda_expression*歸類為匿名函式 ([匿名函式運算式](expressions.md#anonymous-function-expressions))。</span><span class="sxs-lookup"><span data-stu-id="7c308-520">An *anonymous_method_expression* or *lambda_expression* is classified as an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="7c308-521">運算式沒有類型，但可以隱含地轉換成相容的委派型別或運算式樹狀架構型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-521">The expression does not have a type but can be implicitly converted to a compatible delegate type or expression tree type.</span></span> <span data-ttu-id="7c308-522">具體來說，匿名函式`F`與委派型別相容`D`提供：</span><span class="sxs-lookup"><span data-stu-id="7c308-522">Specifically, an anonymous function `F` is compatible with a delegate type `D` provided:</span></span>

*  <span data-ttu-id="7c308-523">如果`F`包含*anonymous_function_signature*，然後`D`和`F`有相同數目的參數。</span><span class="sxs-lookup"><span data-stu-id="7c308-523">If `F` contains an *anonymous_function_signature*, then `D` and `F` have the same number of parameters.</span></span>
*  <span data-ttu-id="7c308-524">如果`F`neobsahuje *anonymous_function_signature*，然後`D`可能有零個或多個參數的任何類型的任何參數只要`D`具有`out`參數修飾詞。</span><span class="sxs-lookup"><span data-stu-id="7c308-524">If `F` does not contain an *anonymous_function_signature*, then `D` may have zero or more parameters of any type, as long as no parameter of `D` has the `out` parameter modifier.</span></span>
*  <span data-ttu-id="7c308-525">如果`F`具有明確類型的參數清單，在每個參數`D`中的對應參數有相同的類型和修飾詞`F`。</span><span class="sxs-lookup"><span data-stu-id="7c308-525">If `F` has an explicitly typed parameter list, each parameter in `D` has the same type and modifiers as the corresponding parameter in `F`.</span></span>
*  <span data-ttu-id="7c308-526">如果`F`具有隱含型別的參數清單，`D`沒有`ref`或`out`參數。</span><span class="sxs-lookup"><span data-stu-id="7c308-526">If `F` has an implicitly typed parameter list, `D` has no `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="7c308-527">如果主體`F`是運算式，且`D`已`void`傳回型別或`F`是非同步和`D`具有傳回型別`Task`，然後當每個參數的`F`指定的型別中對應的參數`D`，本文`F`是有效的運算式 (wrt[運算式](expressions.md))，以允許*statement_expression* ([運算式陳述式](statements.md#expression-statements))。</span><span class="sxs-lookup"><span data-stu-id="7c308-527">If the body of `F` is an expression, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that would be permitted as a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>
*  <span data-ttu-id="7c308-528">如果主體`F`是陳述式區塊，且`D`已`void`傳回型別或`F`是非同步和`D`具有傳回型別`Task`，然後當每個參數的`F`指定的型別中對應的參數`D`，本文`F`是有效的陳述式區塊 (wrt[區塊](statements.md#blocks)) 中且未`return`陳述式指定的運算式。</span><span class="sxs-lookup"><span data-stu-id="7c308-528">If the body of `F` is a statement block, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) in which no `return` statement specifies an expression.</span></span>
*  <span data-ttu-id="7c308-529">如果主體`F`是運算式，並*任一*`F`是非同步和`D`具有非 void 傳回型別`T`，*或*`F`是非同步和`D`的傳回型別`Task<T>`，然後當每個參數`F`中的對應參數的型別`D`，主體`F`是有效的運算式 (wrt [運算式](expressions.md))，會隱含地轉換成`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-529">If the body of `F` is an expression, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that is implicitly convertible to `T`.</span></span>
*  <span data-ttu-id="7c308-530">如果主體`F`陳述式區塊，並*任一*`F`是非同步和`D`具有非 void 傳回型別`T`，*或*`F`是非同步和`D`的傳回型別`Task<T>`，然後當每個參數`F`中的對應參數的型別`D`，主體`F`是有效的陳述式區塊 (wrt[區塊](statements.md#blocks)) 與非可聯繫的端點，其中每一個`return`陳述式指定的運算式，會隱含地轉換成`T`。</span><span class="sxs-lookup"><span data-stu-id="7c308-530">If the body of `F` is a statement block, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) with a non-reachable end point in which each `return` statement specifies an expression that is implicitly convertible to `T`.</span></span>

<span data-ttu-id="7c308-531">為了簡潔起見，本節中，請使用工作類型的簡短形式`Task`並`Task<T>`([非同步函式](classes.md#async-functions))。</span><span class="sxs-lookup"><span data-stu-id="7c308-531">For the purpose of brevity, this section uses the short form for the task types `Task` and `Task<T>` ([Async functions](classes.md#async-functions)).</span></span>

<span data-ttu-id="7c308-532">Lambda 運算式`F`相容的運算式樹狀架構型別`Expression<D>`如果`F`相容於委派型別`D`。</span><span class="sxs-lookup"><span data-stu-id="7c308-532">A lambda expression `F` is compatible with an expression tree type `Expression<D>` if `F` is compatible with the delegate type `D`.</span></span> <span data-ttu-id="7c308-533">請注意這不適用於匿名方法，只是 lambda 運算式。</span><span class="sxs-lookup"><span data-stu-id="7c308-533">Note that this does not apply to anonymous methods, only lambda expressions.</span></span>

<span data-ttu-id="7c308-534">特定的 lambda 運算式無法轉換成運算式樹狀架構類型： 即使轉換*存在*，它在編譯時期就會失敗。</span><span class="sxs-lookup"><span data-stu-id="7c308-534">Certain lambda expressions cannot be converted to expression tree types: Even though the conversion *exists*, it fails at compile-time.</span></span> <span data-ttu-id="7c308-535">這是則 lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="7c308-535">This is the case if the lambda expression:</span></span>

*  <span data-ttu-id="7c308-536">已*區塊*主體</span><span class="sxs-lookup"><span data-stu-id="7c308-536">Has a *block* body</span></span>
*  <span data-ttu-id="7c308-537">包含簡單或複合指派運算子</span><span class="sxs-lookup"><span data-stu-id="7c308-537">Contains simple or compound assignment operators</span></span>
*  <span data-ttu-id="7c308-538">包含動態繫結的運算式</span><span class="sxs-lookup"><span data-stu-id="7c308-538">Contains a dynamically bound expression</span></span>
*  <span data-ttu-id="7c308-539">為非同步</span><span class="sxs-lookup"><span data-stu-id="7c308-539">Is async</span></span>

<span data-ttu-id="7c308-540">請依照下列範例會使用泛型委派型別`Func<A,R>`表示接受類型的引數的函式`A`，並傳回型別的值`R`:</span><span class="sxs-lookup"><span data-stu-id="7c308-540">The examples that follow use a generic delegate type `Func<A,R>` which represents a function that takes an argument of type `A` and returns a value of type `R`:</span></span>
```csharp
delegate R Func<A,R>(A arg);
```

<span data-ttu-id="7c308-541">在工作分派</span><span class="sxs-lookup"><span data-stu-id="7c308-541">In the assignments</span></span>
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
<span data-ttu-id="7c308-542">每個匿名函式的參數和傳回型別是匿名函式會指派給變數的類型決定。</span><span class="sxs-lookup"><span data-stu-id="7c308-542">the parameter and return types of each anonymous function are determined from the type of the variable to which the anonymous function is assigned.</span></span>

<span data-ttu-id="7c308-543">第一次指派成功轉換成委派類型的匿名函式`Func<int,int>`因為，當`x`為 object 型別`int`，`x+1`是有效的運算式，會隱含地轉換成輸入`int`。</span><span class="sxs-lookup"><span data-stu-id="7c308-543">The first assignment successfully converts the anonymous function to the delegate type `Func<int,int>` because, when `x` is given type `int`, `x+1` is a valid expression that is implicitly convertible to type `int`.</span></span>

<span data-ttu-id="7c308-544">同樣地，第二個指派成功會將匿名函式的委派型別`Func<int,double>`因為的結果`x+1`(型別的`int`) 是隱含轉換成類型`double`。</span><span class="sxs-lookup"><span data-stu-id="7c308-544">Likewise, the second assignment successfully converts the anonymous function to the delegate type `Func<int,double>` because the result of `x+1` (of type `int`) is implicitly convertible to type `double`.</span></span>

<span data-ttu-id="7c308-545">不過，第三個的指派是編譯時期錯誤，因為，當`x`為 object 型別`double`，結果`x+1`(型別`double`) 不是隱含轉換成類型`int`。</span><span class="sxs-lookup"><span data-stu-id="7c308-545">However, the third assignment is a compile-time error because, when `x` is given type `double`, the result of `x+1` (of type `double`) is not implicitly convertible to type `int`.</span></span>

<span data-ttu-id="7c308-546">第四個指派成功將匿名的非同步函式轉換成委派類型`Func<int, Task<int>>`因為的結果`x+1`(型別的`int`) 會隱含地轉換成的結果型別`int`型別的工作`Task<int>`.</span><span class="sxs-lookup"><span data-stu-id="7c308-546">The fourth assignment successfully converts the anonymous async function to the delegate type `Func<int, Task<int>>` because the result of `x+1` (of type `int`) is implicitly convertible to the result type `int` of the task type `Task<int>`.</span></span>

<span data-ttu-id="7c308-547">匿名函式可能會影響多載解析，並參與型別推斷。</span><span class="sxs-lookup"><span data-stu-id="7c308-547">Anonymous functions may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="7c308-548">請參閱[函式成員](expressions.md#function-members)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="7c308-548">See [Function members](expressions.md#function-members) for further details.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a><span data-ttu-id="7c308-549">匿名函式轉換成委派類型的評估結果</span><span class="sxs-lookup"><span data-stu-id="7c308-549">Evaluation of anonymous function conversions to delegate types</span></span>

<span data-ttu-id="7c308-550">轉換成委派類型的匿名函式會產生委派執行個體參考的匿名函式和 （可能是空的） 擷取評估時處於作用中的外部變數的集合。</span><span class="sxs-lookup"><span data-stu-id="7c308-550">Conversion of an anonymous function to a delegate type produces a delegate instance which references the anonymous function and the (possibly empty) set of captured outer variables that are active at the time of the evaluation.</span></span> <span data-ttu-id="7c308-551">叫用委派時，會執行匿名函式的主體。</span><span class="sxs-lookup"><span data-stu-id="7c308-551">When the delegate is invoked, the body of the anonymous function is executed.</span></span> <span data-ttu-id="7c308-552">在本文中的程式碼會執行使用一組擷取委派所參考的外部變數。</span><span class="sxs-lookup"><span data-stu-id="7c308-552">The code in the body is executed using the set of captured outer variables referenced by the delegate.</span></span>

<span data-ttu-id="7c308-553">所產生的匿名函式的委派引動過程清單包含單一項目。</span><span class="sxs-lookup"><span data-stu-id="7c308-553">The invocation list of a delegate produced from an anonymous function contains a single entry.</span></span> <span data-ttu-id="7c308-554">確切的目標物件和目標方法的委派都未指定。</span><span class="sxs-lookup"><span data-stu-id="7c308-554">The exact target object and target method of the delegate are unspecified.</span></span> <span data-ttu-id="7c308-555">特別是，它未指定目標物件的委派是否`null`，則`this`封入函式成員或其他某些物件的值。</span><span class="sxs-lookup"><span data-stu-id="7c308-555">In particular, it is unspecified whether the target object of the delegate is `null`, the `this` value of the enclosing function member, or some other object.</span></span>

<span data-ttu-id="7c308-556">轉換相同語意的匿名函式以擷取外部變數執行個體相同的 （可能是空的） 設定為相同的委派類型都允許 （但非必要） 傳回相同的委派執行個體。</span><span class="sxs-lookup"><span data-stu-id="7c308-556">Conversions of semantically identical anonymous functions with the same (possibly empty) set of captured outer variable instances to the same delegate types are permitted (but not required) to return the same delegate instance.</span></span> <span data-ttu-id="7c308-557">語意上完全相同的詞彙是此處用來表示，匿名函式的執行會在所有情況下，產生相同的效果，提供相同的引數。</span><span class="sxs-lookup"><span data-stu-id="7c308-557">The term semantically identical is used here to mean that execution of the anonymous functions will, in all cases, produce the same effects given the same arguments.</span></span> <span data-ttu-id="7c308-558">此規則會允許程式碼，如下所示進行最佳化。</span><span class="sxs-lookup"><span data-stu-id="7c308-558">This rule permits code such as the following to be optimized.</span></span>

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

<span data-ttu-id="7c308-559">由於兩個的匿名函式委派具有相同的 （空的） 集合擷取的外部變數，以及因為匿名函式的語意相同，編譯器允許具有參考相同的目標方法的委派。</span><span class="sxs-lookup"><span data-stu-id="7c308-559">Since the two anonymous function delegates have the same (empty) set of captured outer variables, and since the anonymous functions are semantically identical, the compiler is permitted to have the delegates refer to the same target method.</span></span> <span data-ttu-id="7c308-560">事實上，編譯器可以從這兩個匿名函式運算式中傳回相同的委派執行個體。</span><span class="sxs-lookup"><span data-stu-id="7c308-560">Indeed, the compiler is permitted to return the very same delegate instance from both anonymous function expressions.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a><span data-ttu-id="7c308-561">匿名函式轉換成運算式樹狀架構類型的評估結果</span><span class="sxs-lookup"><span data-stu-id="7c308-561">Evaluation of anonymous function conversions to expression tree types</span></span>

<span data-ttu-id="7c308-562">匿名函式轉換成運算式樹狀架構型別會產生運算式樹狀架構 ([運算式樹狀架構類型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="7c308-562">Conversion of an anonymous function to an expression tree type produces an expression tree ([Expression tree types](types.md#expression-tree-types)).</span></span> <span data-ttu-id="7c308-563">更精確地說，評估的匿名函式轉換會導致建構的物件結構，表示匿名函式本身的結構。</span><span class="sxs-lookup"><span data-stu-id="7c308-563">More precisely, evaluation of the anonymous function conversion leads to the construction of an object structure that represents the structure of the anonymous function itself.</span></span> <span data-ttu-id="7c308-564">運算式樹狀架構，以及用於建立它，確切的程序的精確結構會實作所定義。</span><span class="sxs-lookup"><span data-stu-id="7c308-564">The precise structure of the expression tree, as well as the exact process for creating it, are implementation defined.</span></span>

### <a name="implementation-example"></a><span data-ttu-id="7c308-565">實作範例</span><span class="sxs-lookup"><span data-stu-id="7c308-565">Implementation example</span></span>

<span data-ttu-id="7c308-566">本節說明可能的實作，根據其他 C# 建構的匿名函式轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-566">This section describes a possible implementation of anonymous function conversions in terms of other C# constructs.</span></span> <span data-ttu-id="7c308-567">此處所述的實作根據相同的原則，由 Microsoft C# 編譯器，但它不是託管的實作中，也不是只有一個可能的。</span><span class="sxs-lookup"><span data-stu-id="7c308-567">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation, nor is it the only one possible.</span></span> <span data-ttu-id="7c308-568">它僅簡單提及轉換成運算式樹狀架構，因為其確切語意不超出此規格的範圍。</span><span class="sxs-lookup"><span data-stu-id="7c308-568">It only briefly mentions conversions to expression trees, as their exact semantics are outside the scope of this specification.</span></span>

<span data-ttu-id="7c308-569">本節的其餘部分會提供數個範例包含具有不同特性的匿名函式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7c308-569">The remainder of this section gives several examples of code that contains anonymous functions with different characteristics.</span></span> <span data-ttu-id="7c308-570">對於每個範例中，提供對應的翻譯，使用只有其他 C# 建構程式碼。</span><span class="sxs-lookup"><span data-stu-id="7c308-570">For each example, a corresponding translation to code that uses only other C# constructs is provided.</span></span> <span data-ttu-id="7c308-571">在範例中，識別碼`D`會假設為代表下列委派型別：</span><span class="sxs-lookup"><span data-stu-id="7c308-571">In the examples, the identifier `D` is assumed by represent the following delegate type:</span></span>
```csharp
public delegate void D();
```

<span data-ttu-id="7c308-572">匿名函式的最簡單形式是擷取任何外部變數的其中一個：</span><span class="sxs-lookup"><span data-stu-id="7c308-572">The simplest form of an anonymous function is one that captures no outer variables:</span></span>
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

<span data-ttu-id="7c308-573">這可以轉譯為委派具現化參考編譯器產生的靜態方法，匿名函式的程式碼位於：</span><span class="sxs-lookup"><span data-stu-id="7c308-573">This can be translated to a delegate instantiation that references a compiler generated static method in which the code of the anonymous function is placed:</span></span>
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

<span data-ttu-id="7c308-574">在下列範例中，匿名函式參考的執行個體成員`this`:</span><span class="sxs-lookup"><span data-stu-id="7c308-574">In the following example, the anonymous function references instance members of `this`:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

<span data-ttu-id="7c308-575">這可以轉譯為包含匿名函式的程式碼編譯器產生的執行個體方法：</span><span class="sxs-lookup"><span data-stu-id="7c308-575">This can be translated to a compiler generated instance method containing the code of the anonymous function:</span></span>
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

<span data-ttu-id="7c308-576">在此範例中，匿名函式會擷取區域變數：</span><span class="sxs-lookup"><span data-stu-id="7c308-576">In this example, the anonymous function captures a local variable:</span></span>
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

<span data-ttu-id="7c308-577">本機變數的存留期必須延伸為至少匿名函式委派的存留期。</span><span class="sxs-lookup"><span data-stu-id="7c308-577">The lifetime of the local variable must now be extended to at least the lifetime of the anonymous function delegate.</span></span> <span data-ttu-id="7c308-578">這可藉由編譯器產生類別的欄位"hoisting 」 的本機變數。</span><span class="sxs-lookup"><span data-stu-id="7c308-578">This can be achieved by "hoisting" the local variable into a field of a compiler generated class.</span></span> <span data-ttu-id="7c308-579">具現化的本機變數 ([具現化的本機變數](expressions.md#instantiation-of-local-variables)) 再建立的執行個體的編譯器產生的類別，以及存取本機變數對應至存取的執行個體中的欄位對應編譯器產生的類別。</span><span class="sxs-lookup"><span data-stu-id="7c308-579">Instantiation of the local variable ([Instantiation of local variables](expressions.md#instantiation-of-local-variables)) then corresponds to creating an instance of the compiler generated class, and accessing the local variable corresponds to accessing a field in the instance of the compiler generated class.</span></span> <span data-ttu-id="7c308-580">此外，匿名函式會變成編譯器產生類別的執行個體方法：</span><span class="sxs-lookup"><span data-stu-id="7c308-580">Furthermore, the anonymous function becomes an instance method of the compiler generated class:</span></span>
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

<span data-ttu-id="7c308-581">最後，下列匿名函式擷取`this`以及使用不同的存留期的兩個區域變數：</span><span class="sxs-lookup"><span data-stu-id="7c308-581">Finally, the following anonymous function captures `this` as well as two local variables with different lifetimes:</span></span>
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

<span data-ttu-id="7c308-582">在這裡，建立每個陳述式的編譯器產生類別中的區域變數會擷取讓不同的區塊中的區域變數可以擁有不同的存留期的區塊。</span><span class="sxs-lookup"><span data-stu-id="7c308-582">Here, a compiler generated class is created for each statement block in which locals are captured such that the locals in the different blocks can have independent lifetimes.</span></span> <span data-ttu-id="7c308-583">執行個體`__Locals2`，編譯器產生的類別內部陳述式區塊中，包含本機變數`z`的欄位參考的執行個體，以及`__Locals1`。</span><span class="sxs-lookup"><span data-stu-id="7c308-583">An instance of `__Locals2`, the compiler generated class for the inner statement block, contains the local variable `z` and a field that references an instance of `__Locals1`.</span></span>  <span data-ttu-id="7c308-584">執行個體`__Locals1`，外部陳述式區塊中，編譯器產生的類別會包含區域變數`y`和 [參考] 欄位`this`封入函式成員。</span><span class="sxs-lookup"><span data-stu-id="7c308-584">An instance of `__Locals1`, the compiler generated class for the outer statement block, contains the local variable `y` and a field that references `this` of the enclosing function member.</span></span> <span data-ttu-id="7c308-585">使用這些資料結構，就可以連到所有擷取外部變數的執行個體透過`__Local2`，而匿名函式的程式碼可以因此實作為該類別的執行個體方法。</span><span class="sxs-lookup"><span data-stu-id="7c308-585">With these data structures it is possible to reach all captured outer variables through an instance of `__Local2`, and the code of the anonymous function can thus be implemented as an instance method of that class.</span></span>

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

<span data-ttu-id="7c308-586">這裡套用至擷取本機變數相同的技巧也可以用匿名函式轉換成運算式樹狀架構時： 運算式樹狀結構中儲存編譯器產生物件的參考，而且可以是本機變數的存取權以欄位存取這些物件表示。</span><span class="sxs-lookup"><span data-stu-id="7c308-586">The same technique applied here to capture local variables can also be used when converting anonymous functions to expression trees: References to the compiler generated objects can be stored in the expression tree, and access to the local variables can be represented as field accesses on these objects.</span></span> <span data-ttu-id="7c308-587">此方法的優點是它可讓 「 提取 」 的本機變數，委派和運算式樹狀架構之間共用。</span><span class="sxs-lookup"><span data-stu-id="7c308-587">The advantage of this approach is that it allows the "lifted" local variables to be shared between delegates and expression trees.</span></span>

## <a name="method-group-conversions"></a><span data-ttu-id="7c308-588">方法群組轉換</span><span class="sxs-lookup"><span data-stu-id="7c308-588">Method group conversions</span></span>

<span data-ttu-id="7c308-589">隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions)) 存在從方法群組 ([運算式分類](expressions.md#expression-classifications)) 相容的委派型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-589">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from a method group ([Expression classifications](expressions.md#expression-classifications)) to a compatible delegate type.</span></span> <span data-ttu-id="7c308-590">指定的委派型別`D`和運算式`E`可分類為方法群組、 隱含的轉換`E`要`D`如果`E`包含至少一個的方法，其一般表單 （適用於[適用的函式成員](expressions.md#applicable-function-member)) 引數清單中，建構所使用的參數類型和修飾詞`D`，如下列所述。</span><span class="sxs-lookup"><span data-stu-id="7c308-590">Given a delegate type `D` and an expression `E` that is classified as a method group, an implicit conversion exists from `E` to `D` if `E` contains at least one method that is applicable in its normal form ([Applicable function member](expressions.md#applicable-function-member)) to an argument list constructed by use of the parameter types and modifiers of `D`, as described in the following.</span></span>

<span data-ttu-id="7c308-591">從方法群組轉換的編譯時期應用程式`E`成委派類型`D`下列所述。</span><span class="sxs-lookup"><span data-stu-id="7c308-591">The compile-time application of a conversion from a method group `E` to a delegate type `D` is described in the following.</span></span> <span data-ttu-id="7c308-592">請注意，是否存在的隱含轉換`E`至`D`不保證轉換的編譯時期應用程式將會成功且沒有錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-592">Note that the existence of an implicit conversion from `E` to `D` does not guarantee that the compile-time application of the conversion will succeed without error.</span></span>

*  <span data-ttu-id="7c308-593">單一方法`M`已選取對應至方法的引動過程 ([方法引動過程](expressions.md#method-invocations)) 的形式`E(A)`，進行下列修改：</span><span class="sxs-lookup"><span data-stu-id="7c308-593">A single method `M` is selected corresponding to a method invocation ([Method invocations](expressions.md#method-invocations)) of the form `E(A)`, with the following modifications:</span></span>
    * <span data-ttu-id="7c308-594">引數清單`A`是運算式，每個已分類為變數，且具有類型和修飾詞的清單 (`ref`或是`out`) 中的對應參數*formal_parameter_list* 的`D`.</span><span class="sxs-lookup"><span data-stu-id="7c308-594">The argument list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref` or `out`) of the corresponding parameter in the *formal_parameter_list* of `D`.</span></span>
    * <span data-ttu-id="7c308-595">考慮的候選項目方法為只適用於一般形式的方法 ([適用的函式成員](expressions.md#applicable-function-member))，不列入只適用於其展開的形式。</span><span class="sxs-lookup"><span data-stu-id="7c308-595">The candidate methods considered are only those methods that are applicable in their normal form ([Applicable function member](expressions.md#applicable-function-member)), not those applicable only in their expanded form.</span></span>
*  <span data-ttu-id="7c308-596">如果演算法[方法引動過程](expressions.md#method-invocations)產生錯誤，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c308-596">If the algorithm of [Method invocations](expressions.md#method-invocations) produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="7c308-597">否則，演算法會產生單一的最佳方法`M`具有相同數目的參數做為`D`並轉換不存在。</span><span class="sxs-lookup"><span data-stu-id="7c308-597">Otherwise the algorithm produces a single best method `M` having the same number of parameters as `D` and the conversion is considered to exist.</span></span>
*  <span data-ttu-id="7c308-598">選取的方法`M`必須是相容 ([委派相容性](delegates.md#delegate-compatibility)) 與委派類型`D`，或發生編譯時期錯誤，否則為。</span><span class="sxs-lookup"><span data-stu-id="7c308-598">The selected method `M` must be compatible ([Delegate compatibility](delegates.md#delegate-compatibility)) with the delegate type `D`, or otherwise, a compile-time error occurs.</span></span>
*  <span data-ttu-id="7c308-599">如果選取的方法`M`是執行個體方法，與相關聯的執行個體運算式`E`判斷目標物件的委派。</span><span class="sxs-lookup"><span data-stu-id="7c308-599">If the selected method `M` is an instance method, the instance expression associated with `E` determines the target object of the delegate.</span></span>
*  <span data-ttu-id="7c308-600">如果選取的方法 M 表示透過成員存取運算式的執行個體上的擴充方法，該執行個體運算式會判斷目標物件的委派。</span><span class="sxs-lookup"><span data-stu-id="7c308-600">If the selected method M is an extension method which is denoted by means of a member access on an instance expression, that instance expression determines the target object of the delegate.</span></span>
*  <span data-ttu-id="7c308-601">轉換的結果是型別的值 `D`，也就是指的是所選的方法和目標物件的新建立的委派。</span><span class="sxs-lookup"><span data-stu-id="7c308-601">The result of the conversion is a value of type `D`, namely a newly created delegate that refers to the selected method and target object.</span></span>
*  <span data-ttu-id="7c308-602">請注意，如果此程序會造成的擴充方法，委派建立的演算法[方法引動過程](expressions.md#method-invocations)無法找到的執行個體方法，但成功處理的引動過程`E(A)`作為擴充功能方法引動過程 ([擴充方法引動過程](expressions.md#extension-method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="7c308-602">Note that this process can lead to the creation of a delegate to an extension method, if the algorithm of [Method invocations](expressions.md#method-invocations) fails to find an instance method but succeeds in processing the invocation of `E(A)` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="7c308-603">委派，因此建立擷取的擴充方法，以及其第一個引數。</span><span class="sxs-lookup"><span data-stu-id="7c308-603">A delegate thus created captures the extension method as well as its first argument.</span></span>

<span data-ttu-id="7c308-604">下列範例會示範方法群組轉換：</span><span class="sxs-lookup"><span data-stu-id="7c308-604">The following example demonstrates method group conversions:</span></span>
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

<span data-ttu-id="7c308-605">指派給`d1`會隱含地轉換方法群組`F`類型的值來`D1`。</span><span class="sxs-lookup"><span data-stu-id="7c308-605">The assignment to `d1` implicitly converts the method group `F` to a value of type `D1`.</span></span>

<span data-ttu-id="7c308-606">若要指派`d2`示範如何建立具有較少衍生 (contravariant) 型別參數的方法的委派可以和更多衍生 （共變數） 的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="7c308-606">The assignment to `d2` shows how it is possible to create a delegate to a method that has less derived (contravariant) parameter types and a more derived (covariant) return type.</span></span>

<span data-ttu-id="7c308-607">若要指派`d3`顯示沒有轉換的存在於是否方法不適用。</span><span class="sxs-lookup"><span data-stu-id="7c308-607">The assignment to `d3` shows how no conversion exists if the method is not applicable.</span></span>

<span data-ttu-id="7c308-608">若要指派`d4`顯示方式的方法必須以其標準形式。</span><span class="sxs-lookup"><span data-stu-id="7c308-608">The assignment to `d4` shows how the method must be applicable in its normal form.</span></span>

<span data-ttu-id="7c308-609">若要指派`d5`顯示允許如何與不同只適用於參考類型的委派和方法的參數和傳回類型。</span><span class="sxs-lookup"><span data-stu-id="7c308-609">The assignment to `d5` shows how parameter and return types of the delegate and method are allowed to differ only for reference types.</span></span>

<span data-ttu-id="7c308-610">如同所有其他隱含和明確轉換，轉換運算子可用來明確地執行方法群組轉換。</span><span class="sxs-lookup"><span data-stu-id="7c308-610">As with all other implicit and explicit conversions, the cast operator can be used to explicitly perform a method group conversion.</span></span> <span data-ttu-id="7c308-611">因此，範例</span><span class="sxs-lookup"><span data-stu-id="7c308-611">Thus, the example</span></span>
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
<span data-ttu-id="7c308-612">可以改為撰寫</span><span class="sxs-lookup"><span data-stu-id="7c308-612">could instead be written</span></span>
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

<span data-ttu-id="7c308-613">方法群組可能會影響多載解析，並參與型別推斷。</span><span class="sxs-lookup"><span data-stu-id="7c308-613">Method groups may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="7c308-614">請參閱[函式成員](expressions.md#function-members)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="7c308-614">See [Function members](expressions.md#function-members) for further details.</span></span>

<span data-ttu-id="7c308-615">方法群組轉換執行階段評估程序進行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7c308-615">The run-time evaluation of a method group conversion proceeds as follows:</span></span>

*  <span data-ttu-id="7c308-616">如果在編譯時期選取的方法是執行個體方法，或是存取執行個體方法的擴充方法，委派的目標物件從相關聯的執行個體運算式決定`E`:</span><span class="sxs-lookup"><span data-stu-id="7c308-616">If the method selected at compile-time is an instance method, or it is an extension method which is accessed as an instance method, the target object of the delegate is determined from the instance expression associated with `E`:</span></span>
    * <span data-ttu-id="7c308-617">執行個體運算式評估。</span><span class="sxs-lookup"><span data-stu-id="7c308-617">The instance expression is evaluated.</span></span> <span data-ttu-id="7c308-618">如果此評估會產生例外狀況，則會不執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="7c308-618">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="7c308-619">如果執行個體的運算式*reference_type*，執行個體運算式所計算的值會成為目標物件。</span><span class="sxs-lookup"><span data-stu-id="7c308-619">If the instance expression is of a *reference_type*, the value computed by the instance expression becomes the target object.</span></span> <span data-ttu-id="7c308-620">如果選取的方法是執行個體方法和目標物件是`null`、`System.NullReferenceException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="7c308-620">If the selected method is an instance method and the target object is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="7c308-621">如果執行個體的運算式*value_type*，boxing 作業 ([Boxing 轉換](types.md#boxing-conversions)) 會將值轉換成物件，而此物件會變成目標物件。</span><span class="sxs-lookup"><span data-stu-id="7c308-621">If the instance expression is of a *value_type*, a boxing operation ([Boxing conversions](types.md#boxing-conversions)) is performed to convert the value to an object, and this object becomes the target object.</span></span>
*  <span data-ttu-id="7c308-622">否則選取的方法是靜態方法呼叫的一部分，委派的目標物件`null`。</span><span class="sxs-lookup"><span data-stu-id="7c308-622">Otherwise the selected method is part of a static method call, and the target object of the delegate is `null`.</span></span>
*  <span data-ttu-id="7c308-623">委派類型的新執行個體`D`配置。</span><span class="sxs-lookup"><span data-stu-id="7c308-623">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="7c308-624">如果不是記憶體不足，無法配置新的執行個體，`System.OutOfMemoryException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="7c308-624">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="7c308-625">新的委派執行個體初始化方法在編譯時期所決定的參考，且上述計算的目標物件的參考。</span><span class="sxs-lookup"><span data-stu-id="7c308-625">The new delegate instance is initialized with a reference to the method that was determined at compile-time and a reference to the target object computed above.</span></span>
