---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281966"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="c840c-101">目標型別 `new` 運算式</span><span class="sxs-lookup"><span data-stu-id="c840c-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="c840c-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="c840c-102">[x] Proposed</span></span>
* <span data-ttu-id="c840c-103">[x][原型](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="c840c-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="c840c-104">[] 執行</span><span class="sxs-lookup"><span data-stu-id="c840c-104">[ ] Implementation</span></span>
* <span data-ttu-id="c840c-105">[] 規格</span><span class="sxs-lookup"><span data-stu-id="c840c-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="c840c-106">摘要</span><span class="sxs-lookup"><span data-stu-id="c840c-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="c840c-107">當已知型別時，不需要型別規格。</span><span class="sxs-lookup"><span data-stu-id="c840c-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="c840c-108">動機</span><span class="sxs-lookup"><span data-stu-id="c840c-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="c840c-109">允許欄位初始化，而不復制類型。</span><span class="sxs-lookup"><span data-stu-id="c840c-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="c840c-110">當可以從使用方式推斷時，允許省略類型。</span><span class="sxs-lookup"><span data-stu-id="c840c-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="c840c-111">將物件具現化，而不將型別拼出。</span><span class="sxs-lookup"><span data-stu-id="c840c-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="specification"></a><span data-ttu-id="c840c-112">規格</span><span class="sxs-lookup"><span data-stu-id="c840c-112">Specification</span></span>
[design]: #detailed-design

<span data-ttu-id="c840c-113">*Object_creation_expression*的新語法表單*target_typed_new*接受，其中*類型*是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="c840c-113">A new syntactic form, *target_typed_new* of the *object_creation_expression* is accepted in which the *type* is optional.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

<span data-ttu-id="c840c-114">*Target_typed_new*運算式沒有類型。</span><span class="sxs-lookup"><span data-stu-id="c840c-114">A *target_typed_new* expression does not have a type.</span></span> <span data-ttu-id="c840c-115">不過，有一個新的*物件建立轉換*是從運算式隱含轉換，這是從*target_typed_new*到每個型別。</span><span class="sxs-lookup"><span data-stu-id="c840c-115">However, there is a new *object creation conversion* that is an implicit conversion from expression, that exists from a *target_typed_new* to every type.</span></span>

<span data-ttu-id="c840c-116">假設有目標型別 `T`，如果 `T` 是 `System.Nullable`的實例，則類型 `T0` 會 `T`的基礎類型。</span><span class="sxs-lookup"><span data-stu-id="c840c-116">Given a target type `T`, the type `T0` is `T`'s underlying type if `T` is an instance of `System.Nullable`.</span></span> <span data-ttu-id="c840c-117">否則會 `T``T0`。</span><span class="sxs-lookup"><span data-stu-id="c840c-117">Otherwise `T0` is `T`.</span></span> <span data-ttu-id="c840c-118">轉換成類型 `T` 之*target_typed_new*運算式的意義，與指定 `T0` 做為類型之對應*object_creation_expression*的意義相同。</span><span class="sxs-lookup"><span data-stu-id="c840c-118">The meaning of a *target_typed_new* expression that is converted to the type `T` is the same as the meaning of a corresponding *object_creation_expression* that specifies `T0` as the type.</span></span>

<span data-ttu-id="c840c-119">如果*target_typed_new*當做一元或二元運算子的運算元使用，或如果使用它而不受*物件建立轉換*，則為編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="c840c-119">It is a compile-time error if a *target_typed_new* is used as an operand of a unary or binary operator, or if it is used where it is not subject to an *object creation conversion*.</span></span>

> <span data-ttu-id="c840c-120">**開啟問題：** 我們是否允許委派和元組作為目標型別？</span><span class="sxs-lookup"><span data-stu-id="c840c-120">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="c840c-121">上述規則包括委派（參考型別）和元組（結構型別）。</span><span class="sxs-lookup"><span data-stu-id="c840c-121">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="c840c-122">雖然這兩種類型都是可建構的，但如果類型是 inferable，則可以使用匿名函式或元組常值。</span><span class="sxs-lookup"><span data-stu-id="c840c-122">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="c840c-123">其他</span><span class="sxs-lookup"><span data-stu-id="c840c-123">Miscellaneous</span></span>

<span data-ttu-id="c840c-124">以下是規格的結果：</span><span class="sxs-lookup"><span data-stu-id="c840c-124">The following are consequences of the specification:</span></span>

- <span data-ttu-id="c840c-125">允許 `throw new()` （目標型別為 `System.Exception`）</span><span class="sxs-lookup"><span data-stu-id="c840c-125">`throw new()` is allowed (the target type is `System.Exception`)</span></span>
- <span data-ttu-id="c840c-126">二元運算子不允許使用目標具類型的 `new`。</span><span class="sxs-lookup"><span data-stu-id="c840c-126">Target-typed `new` is not allowed with binary operators.</span></span>
- <span data-ttu-id="c840c-127">當沒有要設為目標的類型時，不允許使用：一元運算子、`foreach`的集合在 `using`中，在 `await` 運算式的解構中，以匿名型別屬性（`new { Prop = new() }`），在 `lock` 語句中，在 `sizeof`的成員存取（`fixed`）的`new().field`語句中，以動態分派的作業（`someDynamic.Method(new())`）表示 `is` 運算子的運算元，做為 `??` 運算子的左運算元。,  ...</span><span class="sxs-lookup"><span data-stu-id="c840c-127">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>
- <span data-ttu-id="c840c-128">也不允許做為 `ref`。</span><span class="sxs-lookup"><span data-stu-id="c840c-128">It is also disallowed as a `ref`.</span></span>
- <span data-ttu-id="c840c-129">下列種類的類型不允許做為轉換的目標</span><span class="sxs-lookup"><span data-stu-id="c840c-129">The following kinds of types are not permitted as targets of the conversion</span></span>
  - <span data-ttu-id="c840c-130">**列舉類型：** `new()` 將可運作（因為 `new Enum()` 會提供預設值），但 `new(1)` 將無法運作，因為列舉類型沒有任何方法。</span><span class="sxs-lookup"><span data-stu-id="c840c-130">**Enum types:** `new()` will work (as `new Enum()` works to give the default value), but `new(1)` will not work as enum types do not have a constructor.</span></span>
  - <span data-ttu-id="c840c-131">**介面類別型：** 這適用于 COM 類型的對應建立運算式。</span><span class="sxs-lookup"><span data-stu-id="c840c-131">**Interface types:** This would work the same as the corresponding creation expression for COM types.</span></span>
  - <span data-ttu-id="c840c-132">**陣列類型：** 陣列需要特殊的語法來提供長度。</span><span class="sxs-lookup"><span data-stu-id="c840c-132">**Array types:** arrays need a special syntax to provide the length.</span></span>    
  - <span data-ttu-id="c840c-133">**動態：** 我們不允許 `new dynamic()`，因此不允許以 `dynamic` 作為目標型別的 `new()`。</span><span class="sxs-lookup"><span data-stu-id="c840c-133">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>
  - <span data-ttu-id="c840c-134">**元組：** 這些與使用基礎類型建立物件的意義相同。</span><span class="sxs-lookup"><span data-stu-id="c840c-134">**tuples:** These have the same meaning as an object creation using the underlying type.</span></span>
  - <span data-ttu-id="c840c-135">也會排除*object_creation_expression*中不允許的所有其他類型，例如指標類型。</span><span class="sxs-lookup"><span data-stu-id="c840c-135">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>   

## <a name="drawbacks"></a><span data-ttu-id="c840c-136">缺點</span><span class="sxs-lookup"><span data-stu-id="c840c-136">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="c840c-137">有一些關於目標型別的問題 `new` 建立新的重大變更分類，但我們已經有 `null` 和 `default`的問題，而且這一點也不是重大的問題。</span><span class="sxs-lookup"><span data-stu-id="c840c-137">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="c840c-138">替代方案</span><span class="sxs-lookup"><span data-stu-id="c840c-138">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="c840c-139">大部分關於類型在欄位初始化中無法重複的抱怨，都是關於類型*引數*，而不是類型本身，我們可以只推斷類型引數，例如 `new Dictionary(...)` （或類似），並從引數或集合初始化運算式在本機推斷類型引數。</span><span class="sxs-lookup"><span data-stu-id="c840c-139">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="c840c-140">問題</span><span class="sxs-lookup"><span data-stu-id="c840c-140">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="c840c-141">是否應該禁止運算式樹狀架構中的使用方式？</span><span class="sxs-lookup"><span data-stu-id="c840c-141">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="c840c-142">不要</span><span class="sxs-lookup"><span data-stu-id="c840c-142">(no)</span></span>
- <span data-ttu-id="c840c-143">功能如何與 `dynamic` 引數互動？</span><span class="sxs-lookup"><span data-stu-id="c840c-143">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="c840c-144">（無特殊處理）</span><span class="sxs-lookup"><span data-stu-id="c840c-144">(no special treatment)</span></span>
- <span data-ttu-id="c840c-145">IntelliSense 應該如何與 `new()`搭配使用？</span><span class="sxs-lookup"><span data-stu-id="c840c-145">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="c840c-146">（只有在只有單一目標型別時）</span><span class="sxs-lookup"><span data-stu-id="c840c-146">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="c840c-147">設計會議</span><span class="sxs-lookup"><span data-stu-id="c840c-147">Design meetings</span></span>

- [<span data-ttu-id="c840c-148">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="c840c-148">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="c840c-149">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="c840c-149">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="c840c-150">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="c840c-150">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="c840c-151">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="c840c-151">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="c840c-152">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="c840c-152">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [<span data-ttu-id="c840c-153">LDM-2020-03-25</span><span class="sxs-lookup"><span data-stu-id="c840c-153">LDM-2020-03-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
