---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488834"
---
# <a name="arrays"></a><span data-ttu-id="4a0b4-101">陣列</span><span class="sxs-lookup"><span data-stu-id="4a0b4-101">Arrays</span></span>

<span data-ttu-id="4a0b4-102">陣列是資料結構，其中包含數個透過計算索引存取的變數。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-102">An array is a data structure that contains a number of variables which are accessed through computed indices.</span></span> <span data-ttu-id="4a0b4-103">陣列，也稱為陣列的項目中包含的變數都相同的型別，以及這個型別稱為陣列的項目類型。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-103">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="4a0b4-104">陣列的陣序規範用來決定每個陣列項目相關聯的索引鍵的數目。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-104">An array has a rank which determines the number of indices associated with each array element.</span></span> <span data-ttu-id="4a0b4-105">陣列的陣序規範也稱為陣列的維度。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-105">The rank of an array is also referred to as the dimensions of the array.</span></span> <span data-ttu-id="4a0b4-106">陣列陣序規範的其中一個稱為***維陣列***。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-106">An array with a rank of one is called a ***single-dimensional array***.</span></span> <span data-ttu-id="4a0b4-107">陣列陣序規範大於其中一個稱為***多維陣列***。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-107">An array with a rank greater than one is called a ***multi-dimensional array***.</span></span> <span data-ttu-id="4a0b4-108">特定大小的多維度陣列通常稱為二維陣列，三維陣列，以及等等。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-108">Specific sized multi-dimensional arrays are often referred to as two-dimensional arrays, three-dimensional arrays, and so on.</span></span>

<span data-ttu-id="4a0b4-109">陣列的每個維度具有相關聯的長度，也就是大於或等於零的整數。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-109">Each dimension of an array has an associated length which is an integral number greater than or equal to zero.</span></span> <span data-ttu-id="4a0b4-110">維度長度不是陣列型別的一部分，但而不是建立陣列型別的執行個體建立在執行階段時。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-110">The dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run-time.</span></span> <span data-ttu-id="4a0b4-111">維度的長度將決定該維度的索引的有效範圍：維度的長度`N`，索引的範圍可以從`0`到`N - 1`（含)。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-111">The length of a dimension determines the valid range of indices for that dimension: For a dimension of length `N`, indices can range from `0` to `N - 1` inclusive.</span></span> <span data-ttu-id="4a0b4-112">陣列中的元素總數是產品之陣列中每個維度的長度。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-112">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="4a0b4-113">如果一或多個維度的陣列擁有長度為零，陣列即為空白。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-113">If one or more of the dimensions of an array have a length of zero, the array is said to be empty.</span></span>

<span data-ttu-id="4a0b4-114">陣列的元素型別可以是任一型別，包括陣列型別。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-114">The element type of an array can be any type, including an array type.</span></span>

## <a name="array-types"></a><span data-ttu-id="4a0b4-115">陣列型別</span><span class="sxs-lookup"><span data-stu-id="4a0b4-115">Array types</span></span>

<span data-ttu-id="4a0b4-116">陣列類型以寫入*non_array_type*後面接著一或多個*rank_specifier*s:</span><span class="sxs-lookup"><span data-stu-id="4a0b4-116">An array type is written as a *non_array_type* followed by one or more *rank_specifier*s:</span></span>

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

<span data-ttu-id="4a0b4-117">A *non_array_type*可以是任何*型別*也就是本身並非*array_type*。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-117">A *non_array_type* is any *type* that is not itself an *array_type*.</span></span>

<span data-ttu-id="4a0b4-118">陣列類型的陣序規範根據最左邊*rank_specifier*中*array_type*:A *rank_specifier*表示陣列的陣列，陣序規範的數目加一，「`,`"的語彙基元*rank_specifier*。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-118">The rank of an array type is given by the leftmost *rank_specifier* in the *array_type*: A *rank_specifier* indicates that the array is an array with a rank of one plus the number of "`,`" tokens in the *rank_specifier*.</span></span>

<span data-ttu-id="4a0b4-119">陣列類型的項目類型是類型所產生的刪除最左邊*rank_specifier*:</span><span class="sxs-lookup"><span data-stu-id="4a0b4-119">The element type of an array type is the type that results from deleting the leftmost *rank_specifier*:</span></span>

*  <span data-ttu-id="4a0b4-120">陣列類型的表單`T[R]`是陣列陣序規範`R`和非陣列元素型別`T`。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-120">An array type of the form `T[R]` is an array with rank `R` and a non-array element type `T`.</span></span>
*  <span data-ttu-id="4a0b4-121">陣列類型的表單`T[R][R1]...[Rn]`是陣列陣序規範`R`和 項目類型`T[R1]...[Rn]`。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-121">An array type of the form `T[R][R1]...[Rn]` is an array with rank `R` and an element type `T[R1]...[Rn]`.</span></span>

<span data-ttu-id="4a0b4-122">實際上*rank_specifier*s 會從左向右讀取前的最後一個非陣列項目的類型。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-122">In effect, the *rank_specifier*s are read from left to right before the final non-array element type.</span></span> <span data-ttu-id="4a0b4-123">型別`int[][,,][,]`是一維陣列的二維陣列的三維陣列`int`。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-123">The type `int[][,,][,]` is a single-dimensional array of three-dimensional arrays of two-dimensional arrays of `int`.</span></span>

<span data-ttu-id="4a0b4-124">在執行階段陣列型別的值可以是`null`或該陣列型別的執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-124">At run-time, a value of an array type can be `null` or a reference to an instance of that array type.</span></span>

### <a name="the-systemarray-type"></a><span data-ttu-id="4a0b4-125">System.Array 型別</span><span class="sxs-lookup"><span data-stu-id="4a0b4-125">The System.Array type</span></span>

<span data-ttu-id="4a0b4-126">型別`System.Array`是所有陣列類型的抽象基底類型。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-126">The type `System.Array` is the abstract base type of all array types.</span></span> <span data-ttu-id="4a0b4-127">隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 存在從任何陣列型別`System.Array`，並明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions)) 是否存在從`System.Array`任何陣列類型。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-127">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from any array type to `System.Array`, and an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `System.Array` to any array type.</span></span> <span data-ttu-id="4a0b4-128">請注意，`System.Array`本身並不會*array_type*。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-128">Note that `System.Array` is not itself an *array_type*.</span></span> <span data-ttu-id="4a0b4-129">而是*class_type*所有*array_type*s 衍生。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-129">Rather, it is a *class_type* from which all *array_type*s are derived.</span></span>

<span data-ttu-id="4a0b4-130">在執行階段，類型的值`System.Array`可以是`null`或任何陣列類型的執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-130">At run-time, a value of type `System.Array` can be `null` or a reference to an instance of any array type.</span></span>

### <a name="arrays-and-the-generic-ilist-interface"></a><span data-ttu-id="4a0b4-131">陣列和泛型 IList 介面</span><span class="sxs-lookup"><span data-stu-id="4a0b4-131">Arrays and the generic IList interface</span></span>

<span data-ttu-id="4a0b4-132">一維陣列`T[]`實作介面`System.Collections.Generic.IList<T>`(`IList<T>`簡稱) 和其基底介面。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-132">A one-dimensional array `T[]` implements the interface `System.Collections.Generic.IList<T>` (`IList<T>` for short) and its base interfaces.</span></span> <span data-ttu-id="4a0b4-133">因此，沒有從的隱含轉換`T[]`至`IList<T>`和其基底介面。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-133">Accordingly, there is an implicit conversion from `T[]` to `IList<T>` and its base interfaces.</span></span> <span data-ttu-id="4a0b4-134">此外，如果沒有從隱含參考轉換`S`要`T`再`S[]`實作`IList<T>`，而且沒有從隱含參考轉換`S[]`至`IList<T>`和其基底介面 （[隱含參考轉換](conversions.md#implicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-134">In addition, if there is an implicit reference conversion from `S` to `T` then `S[]` implements `IList<T>` and there is an implicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="4a0b4-135">如果沒有明確參考轉換從`S`來`T`的明確參考轉換就`S[]`來`IList<T>`及其基底介面 ([明確參考轉換](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="4a0b4-135">If there is an explicit reference conversion from `S` to `T` then there is an explicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="4a0b4-136">例如: </span><span class="sxs-lookup"><span data-stu-id="4a0b4-136">For example:</span></span>
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

<span data-ttu-id="4a0b4-137">工作分派`lst2 = oa1`從轉換後就會產生編譯時期錯誤`object[]`到`IList<string>`是明確的轉換，不隱含。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-137">The assignment `lst2 = oa1` generates a compile-time error since the conversion from `object[]` to `IList<string>` is an explicit conversion, not implicit.</span></span> <span data-ttu-id="4a0b4-138">轉型`(IList<string>)oa1`將會在之後的執行時期擲回的例外狀況`oa1`參考`object[]`而非`string[]`。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-138">The cast `(IList<string>)oa1` will cause an exception to be thrown at run-time since `oa1` references an `object[]` and not a `string[]`.</span></span> <span data-ttu-id="4a0b4-139">不過轉型`(IList<string>)oa2`不會因為擲回例外狀況`oa2`參考`string[]`。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-139">However the cast `(IList<string>)oa2` will not cause an exception to be thrown since `oa2` references a `string[]`.</span></span>

<span data-ttu-id="4a0b4-140">每當沒有隱含或明確參考轉換`S[]`來`IList<T>`，也是從明確參考轉換`IList<T>`和其基底介面`S[]`([明確參考轉換](conversions.md#explicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-140">Whenever there is an implicit or explicit reference conversion from `S[]` to `IList<T>`, there is also an explicit reference conversion from `IList<T>` and its base interfaces to `S[]` ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span>

<span data-ttu-id="4a0b4-141">當陣列型別`S[]`實作`IList<T>`，某些實作介面的成員可能會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-141">When an array type `S[]` implements `IList<T>`, some of the members of the implemented interface may throw exceptions.</span></span> <span data-ttu-id="4a0b4-142">精確的行為的介面的實作已超出此規格的範圍。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-142">The precise behavior of the implementation of the interface is beyond the scope of this specification.</span></span>

## <a name="array-creation"></a><span data-ttu-id="4a0b4-143">建立陣列</span><span class="sxs-lookup"><span data-stu-id="4a0b4-143">Array creation</span></span>

<span data-ttu-id="4a0b4-144">藉由建立陣列執行個體*array_creation_expression*s ([陣列建立運算式](expressions.md#array-creation-expressions)) 或欄位或區域變數宣告包含*array_initializer*([陣列初始設定式](arrays.md#array-initializers))。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-144">Array instances are created by *array_creation_expression*s ([Array creation expressions](expressions.md#array-creation-expressions)) or by field or local variable declarations that include an *array_initializer* ([Array initializers](arrays.md#array-initializers)).</span></span>

<span data-ttu-id="4a0b4-145">建立陣列執行個體時，順位和每個維度的長度所建立，然後保持不變執行個體的整個存留期間。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-145">When an array instance is created, the rank and length of each dimension are established and then remain constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="4a0b4-146">換句話說，就無法變更現有的陣列執行個體的陣序，也不是可調整大小及其維度。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-146">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

<span data-ttu-id="4a0b4-147">陣列執行個體一律會是陣列型別。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-147">An array instance is always of an array type.</span></span> <span data-ttu-id="4a0b4-148">`System.Array`型別是抽象類型無法具現化。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-148">The `System.Array` type is an abstract type that cannot be instantiated.</span></span>

<span data-ttu-id="4a0b4-149">所建立的陣列的元素*array_creation_expression*s 一律會初始化為其預設值 ([預設值](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-149">Elements of arrays created by *array_creation_expression*s are always initialized to their default value ([Default values](variables.md#default-values)).</span></span>

## <a name="array-element-access"></a><span data-ttu-id="4a0b4-150">陣列元素存取</span><span class="sxs-lookup"><span data-stu-id="4a0b4-150">Array element access</span></span>

<span data-ttu-id="4a0b4-151">陣列項目用來存取*element_access*運算式 ([陣列存取](expressions.md#array-access)) 的形式`A[I1, I2, ..., In]`，其中`A`是陣列型別和每個運算式`Ix`是類型的運算式`int`， `uint`， `long`， `ulong`，或可以隱含地轉換為一或多個這些型別。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-151">Array elements are accessed using *element_access* expressions ([Array access](expressions.md#array-access)) of the form `A[I1, I2, ..., In]`, where `A` is an expression of an array type and each `Ix` is an expression of type `int`, `uint`, `long`, `ulong`, or can be implicitly converted to one or more of these types.</span></span> <span data-ttu-id="4a0b4-152">陣列元素存取的結果是一個變數，也就是選取索引的陣列元素。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-152">The result of an array element access is a variable, namely the array element selected by the indices.</span></span>

<span data-ttu-id="4a0b4-153">陣列的項目可以使用列舉`foreach`陳述式 ([foreach 陳述式](statements.md#the-foreach-statement))。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-153">The elements of an array can be enumerated using a `foreach` statement ([The foreach statement](statements.md#the-foreach-statement)).</span></span>

## <a name="array-members"></a><span data-ttu-id="4a0b4-154">陣列成員</span><span class="sxs-lookup"><span data-stu-id="4a0b4-154">Array members</span></span>

<span data-ttu-id="4a0b4-155">每個陣列型別會繼承由宣告成員`System.Array`型別。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-155">Every array type inherits the members declared by the `System.Array` type.</span></span>

## <a name="array-covariance"></a><span data-ttu-id="4a0b4-156">陣列共變數</span><span class="sxs-lookup"><span data-stu-id="4a0b4-156">Array covariance</span></span>

<span data-ttu-id="4a0b4-157">任何兩個*reference_type*s`A`並`B`，如果隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 或明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions)) 從`A`來`B`，然後從陣列類型也存在相同的參考轉換`A[R]`為陣列型別`B[R]`，其中`R`的話給定*rank_specifier* （但相同陣列型別）。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-157">For any two *reference_type*s `A` and `B`, if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `A` to `B`, then the same reference conversion also exists from the array type `A[R]` to the array type `B[R]`, where `R` is any given *rank_specifier* (but the same for both array types).</span></span> <span data-ttu-id="4a0b4-158">此關聯性就所謂***陣列共變數***。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-158">This relationship is known as ***array covariance***.</span></span> <span data-ttu-id="4a0b4-159">陣列共變數特別表示陣列型別的值`A[R]`實際上可能是陣列型別的執行個體的參考`B[R]`，如果隱含參考轉換存在`B`至`A`。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-159">Array covariance in particular means that a value of an array type `A[R]` may actually be a reference to an instance of an array type `B[R]`, provided an implicit reference conversion exists from `B` to `A`.</span></span>

<span data-ttu-id="4a0b4-160">陣列共變數，因為指派給參考型別陣列的項目會包含執行階段檢查可確保指派給陣列元素的值實際上是允許的型別 ([簡單指派](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-160">Because of array covariance, assignments to elements of reference type arrays include a run-time check which ensures that the value being assigned to the array element is actually of a permitted type ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="4a0b4-161">例如: </span><span class="sxs-lookup"><span data-stu-id="4a0b4-161">For example:</span></span>
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

<span data-ttu-id="4a0b4-162">指派給`array[i]`中`Fill`方法會隱含地包含執行階段檢查可確保所參考的物件`value`是`null`實際的項目類型相容的執行個體或`array`.</span><span class="sxs-lookup"><span data-stu-id="4a0b4-162">The assignment to `array[i]` in the `Fill` method implicitly includes a run-time check which ensures that the object referenced by `value` is either `null` or an instance that is compatible with the actual element type of `array`.</span></span> <span data-ttu-id="4a0b4-163">在 `Main`的前兩個引動過程`Fill`成功，但第三個引動過程會導致`System.ArrayTypeMismatchException`擲回時執行的第一個指派`array[i]`。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-163">In `Main`, the first two invocations of `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` to be thrown upon executing the first assignment to `array[i]`.</span></span> <span data-ttu-id="4a0b4-164">因為發生例外狀況 boxed`int`無法儲存在`string`陣列。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-164">The exception occurs because a boxed `int` cannot be stored in a `string` array.</span></span>

<span data-ttu-id="4a0b4-165">陣列共變數特別不會延伸到的陣列*value_type*s。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-165">Array covariance specifically does not extend to arrays of *value_type*s.</span></span> <span data-ttu-id="4a0b4-166">例如，沒有任何轉換存在該允許`int[]`視為`object[]`。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-166">For example, no conversion exists that permits an `int[]` to be treated as an `object[]`.</span></span>

## <a name="array-initializers"></a><span data-ttu-id="4a0b4-167">陣列初始設定式</span><span class="sxs-lookup"><span data-stu-id="4a0b4-167">Array initializers</span></span>

<span data-ttu-id="4a0b4-168">陣列初始設定式可能會在欄位宣告中指定 ([欄位](classes.md#fields))，本機變數宣告 ([區域變數宣告](statements.md#local-variable-declarations))，和陣列建立運算式 ([陣列建立運算式](expressions.md#array-creation-expressions)):</span><span class="sxs-lookup"><span data-stu-id="4a0b4-168">Array initializers may be specified in field declarations ([Fields](classes.md#fields)), local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), and array creation expressions ([Array creation expressions](expressions.md#array-creation-expressions)):</span></span>

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="4a0b4-169">陣列初始設定式所組成的變數初始設定式，用括住的一連串 「`{`"和"`}`「 語彙基元和分隔 」`,`"語彙基元。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-169">An array initializer consists of a sequence of variable initializers, enclosed by "`{`" and "`}`" tokens and separated by "`,`" tokens.</span></span> <span data-ttu-id="4a0b4-170">每個變數的初始設定式是一種運算式或在多維度陣列，巢狀的陣列初始設定式的情況下。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-170">Each variable initializer is an expression or, in the case of a multi-dimensional array, a nested array initializer.</span></span>

<span data-ttu-id="4a0b4-171">其中的陣列初始設定式會使用內容決定要初始化之陣列的類型。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-171">The context in which an array initializer is used determines the type of the array being initialized.</span></span> <span data-ttu-id="4a0b4-172">陣列建立運算式，在陣列型別，立即之前的初始設定式，或從陣列初始設定式中的運算式推斷進行相關的設定。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-172">In an array creation expression, the array type immediately precedes the initializer, or is inferred from the expressions in the array initializer.</span></span> <span data-ttu-id="4a0b4-173">在欄位或變數宣告中，陣列型別是欄位或所宣告變數的類型。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-173">In a field or variable declaration, the array type is the type of the field or variable being declared.</span></span> <span data-ttu-id="4a0b4-174">陣列初始設定式中使用時的欄位或變數宣告，例如：</span><span class="sxs-lookup"><span data-stu-id="4a0b4-174">When an array initializer is used in a field or variable declaration, such as:</span></span>
```csharp
int[] a = {0, 2, 4, 6, 8};
```
<span data-ttu-id="4a0b4-175">它是簡略的對等陣列建立運算式：</span><span class="sxs-lookup"><span data-stu-id="4a0b4-175">it is simply shorthand for an equivalent array creation expression:</span></span>
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

<span data-ttu-id="4a0b4-176">一維陣列，陣列初始設定式必須包含一系列的指派相容於陣列的項目類型的運算式。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-176">For a single-dimensional array, the array initializer must consist of a sequence of expressions that are assignment compatible with the element type of the array.</span></span> <span data-ttu-id="4a0b4-177">運算式，初始化陣列元素，以遞增次序順序，從索引零處的項目。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-177">The expressions initialize array elements in increasing order, starting with the element at index zero.</span></span> <span data-ttu-id="4a0b4-178">陣列初始設定式中的運算式數目會決定所建立的陣列執行個體的長度。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-178">The number of expressions in the array initializer determines the length of the array instance being created.</span></span> <span data-ttu-id="4a0b4-179">例如，上述的陣列初始設定式會建立`int[]`長度為 5 的執行個體，然後初始化的執行個體，使用下列值：</span><span class="sxs-lookup"><span data-stu-id="4a0b4-179">For example, the array initializer above creates an `int[]` instance of length 5 and then initializes the instance with the following values:</span></span>
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

<span data-ttu-id="4a0b4-180">多維陣列，陣列初始設定式必須有多個層級的巢狀陣列中的維度相同。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-180">For a multi-dimensional array, the array initializer must have as many levels of nesting as there are dimensions in the array.</span></span> <span data-ttu-id="4a0b4-181">最外層巢狀層級對應至最左邊的維度，最內層的巢狀層級對應於最右側的維度。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-181">The outermost nesting level corresponds to the leftmost dimension and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="4a0b4-182">陣列的每個維度的長度取決於對應的巢狀層級的陣列初始設定式中的項目數。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-182">The length of each dimension of the array is determined by the number of elements at the corresponding nesting level in the array initializer.</span></span> <span data-ttu-id="4a0b4-183">每個巢狀的陣列初始設定式中，項目數目必須與其他陣列初始設定式在相同的層級相同。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-183">For each nested array initializer, the number of elements must be the same as the other array initializers at the same level.</span></span> <span data-ttu-id="4a0b4-184">範例：</span><span class="sxs-lookup"><span data-stu-id="4a0b4-184">The example:</span></span>
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
<span data-ttu-id="4a0b4-185">建立二維陣列的長度為五個最左邊的維度與長度為兩個最右邊的維度：</span><span class="sxs-lookup"><span data-stu-id="4a0b4-185">creates a two-dimensional array with a length of five for the leftmost dimension and a length of two for the rightmost dimension:</span></span>
```csharp
int[,] b = new int[5, 2];
```
<span data-ttu-id="4a0b4-186">然後初始化陣列執行個體和使用下列值：</span><span class="sxs-lookup"><span data-stu-id="4a0b4-186">and then initializes the array instance with the following values:</span></span>
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

<span data-ttu-id="4a0b4-187">如果維度以外的最右邊長度為零，會假設後續的維度，也有長度為零。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-187">If a dimension other than the rightmost is given with length zero, the subsequent dimensions are assumed to also have length zero.</span></span> <span data-ttu-id="4a0b4-188">範例：</span><span class="sxs-lookup"><span data-stu-id="4a0b4-188">The example:</span></span>
```csharp
int[,] c = {};
```
<span data-ttu-id="4a0b4-189">建立二維陣列的長度為零，最左邊和右邊的維度：</span><span class="sxs-lookup"><span data-stu-id="4a0b4-189">creates a two-dimensional array with a length of zero for both the leftmost and the rightmost dimension:</span></span>
```csharp
int[,] c = new int[0, 0];
```

<span data-ttu-id="4a0b4-190">當陣列建立運算式包括明確的維度長度及陣列初始設定式時，長度必須是常數運算式，並在每個巢狀層級的項目數必須符合相對應維度的長度。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-190">When an array creation expression includes both explicit dimension lengths and an array initializer, the lengths must be constant expressions and the number of elements at each nesting level must match the corresponding dimension length.</span></span> <span data-ttu-id="4a0b4-191">以下是一些範例：</span><span class="sxs-lookup"><span data-stu-id="4a0b4-191">Here are some examples:</span></span>
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

<span data-ttu-id="4a0b4-192">這裡的初始設定式`y`導致編譯時期錯誤，因為維度長度運算式不是常數，與的初始設定式`z`導致編譯時期錯誤，因為長度和中的項目數不同意初始設定式。</span><span class="sxs-lookup"><span data-stu-id="4a0b4-192">Here, the initializer for `y` results in a compile-time error because the dimension length expression is not a constant, and the initializer for `z` results in a compile-time error because the length and the number of elements in the initializer do not agree.</span></span>
