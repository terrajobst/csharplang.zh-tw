---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484535"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="870d4-101">目標型別 `new` 運算式</span><span class="sxs-lookup"><span data-stu-id="870d4-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="870d4-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="870d4-102">[x] Proposed</span></span>
* <span data-ttu-id="870d4-103">[x][原型](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="870d4-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="870d4-104">[] 執行</span><span class="sxs-lookup"><span data-stu-id="870d4-104">[ ] Implementation</span></span>
* <span data-ttu-id="870d4-105">[] 規格</span><span class="sxs-lookup"><span data-stu-id="870d4-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="870d4-106">摘要</span><span class="sxs-lookup"><span data-stu-id="870d4-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="870d4-107">當已知型別時，不需要型別規格。</span><span class="sxs-lookup"><span data-stu-id="870d4-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="870d4-108">動機</span><span class="sxs-lookup"><span data-stu-id="870d4-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="870d4-109">允許欄位初始化，而不復制類型。</span><span class="sxs-lookup"><span data-stu-id="870d4-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
<span data-ttu-id="870d4-110">當可以從使用方式推斷時，允許省略類型。</span><span class="sxs-lookup"><span data-stu-id="870d4-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
<span data-ttu-id="870d4-111">將物件具現化，而不將型別拼出。</span><span class="sxs-lookup"><span data-stu-id="870d4-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a><span data-ttu-id="870d4-112">詳細設計</span><span class="sxs-lookup"><span data-stu-id="870d4-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="870d4-113">當有括弧時，會修改*object_creation_expression*語法，讓*類型*成為選擇性。</span><span class="sxs-lookup"><span data-stu-id="870d4-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="870d4-114">這是解決*anonymous_object_creation_expression*不明確的必要工作。</span><span class="sxs-lookup"><span data-stu-id="870d4-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
<span data-ttu-id="870d4-115">目標具類型的 `new` 可以轉換成任何類型。</span><span class="sxs-lookup"><span data-stu-id="870d4-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="870d4-116">因此，它不會影響多載解析。</span><span class="sxs-lookup"><span data-stu-id="870d4-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="870d4-117">這主要是為了避免無法預期的重大變更。</span><span class="sxs-lookup"><span data-stu-id="870d4-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="870d4-118">在決定型別之後，將會系結引數清單和初始化運算式運算式。</span><span class="sxs-lookup"><span data-stu-id="870d4-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="870d4-119">運算式的型別是從目標型別推斷而來，必須是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="870d4-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="870d4-120">**任何結構類型**</span><span class="sxs-lookup"><span data-stu-id="870d4-120">**Any struct type**</span></span>
- <span data-ttu-id="870d4-121">**任何參考型別**</span><span class="sxs-lookup"><span data-stu-id="870d4-121">**Any reference type**</span></span>
- <span data-ttu-id="870d4-122">具有函數或 `struct` 條件約束的**任何類型參數**</span><span class="sxs-lookup"><span data-stu-id="870d4-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="870d4-123">但有下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="870d4-123">with the following exceptions:</span></span>

- <span data-ttu-id="870d4-124">**列舉類型：** 並非所有列舉類型都包含常數零，因此最好使用明確列舉成員。</span><span class="sxs-lookup"><span data-stu-id="870d4-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="870d4-125">**介面類別型：** 這是一項小規模功能，最好是明確提及該類型。</span><span class="sxs-lookup"><span data-stu-id="870d4-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="870d4-126">**陣列類型：** 陣列需要特殊的語法來提供長度。</span><span class="sxs-lookup"><span data-stu-id="870d4-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="870d4-127">**結構預設**的函式：此規則會排除所有基本型別和大部分的實值型別。</span><span class="sxs-lookup"><span data-stu-id="870d4-127">**Struct default constructor**: this rules out all primitive types and most value types.</span></span> <span data-ttu-id="870d4-128">如果您想要使用這類類型的預設值，您可以改為撰寫 `default`。</span><span class="sxs-lookup"><span data-stu-id="870d4-128">If you wanted to use the default value of such types you could write `default` instead.</span></span>

<span data-ttu-id="870d4-129">也會排除*object_creation_expression*中不允許的所有其他類型，例如指標類型。</span><span class="sxs-lookup"><span data-stu-id="870d4-129">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

> <span data-ttu-id="870d4-130">**開啟問題：** 我們是否允許委派和元組作為目標型別？</span><span class="sxs-lookup"><span data-stu-id="870d4-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="870d4-131">上述規則包括委派（參考型別）和元組（結構型別）。</span><span class="sxs-lookup"><span data-stu-id="870d4-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="870d4-132">雖然這兩種類型都是可建構的，但如果類型是 inferable，則可以使用匿名函式或元組常值。</span><span class="sxs-lookup"><span data-stu-id="870d4-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> <span data-ttu-id="870d4-133">**開啟問題：** 我們是否允許以 `Exception` 的 `throw new()` 做為目標型別？</span><span class="sxs-lookup"><span data-stu-id="870d4-133">**Open Issue:** should we allow `throw new()` with `Exception` as the target-type?</span></span>

<span data-ttu-id="870d4-134">我們目前已 `throw null`，但不 `throw default` （雖然會有相同的效果）。</span><span class="sxs-lookup"><span data-stu-id="870d4-134">We have `throw null` today, but not `throw default` (though it would have the same effect).</span></span> <span data-ttu-id="870d4-135">另一方面，`throw new()` 可以做為 `throw new Exception(...)`的縮寫。</span><span class="sxs-lookup"><span data-stu-id="870d4-135">On the other hand, `throw new()` could be actually useful as a shorthand for `throw new Exception(...)`.</span></span> <span data-ttu-id="870d4-136">請注意，目前的規格已允許此檔案。</span><span class="sxs-lookup"><span data-stu-id="870d4-136">Note that it is already allowed by the current specification.</span></span> <span data-ttu-id="870d4-137">`Exception` 是參考型別，而 throw 語句的規格指出運算式會轉換成 `Exception`。</span><span class="sxs-lookup"><span data-stu-id="870d4-137">`Exception` is a reference type, and the specification for the throw statement says that the expression is converted to `Exception`.</span></span>

> <span data-ttu-id="870d4-138">**開啟問題：** 我們是否允許使用目標型別的 `new` 搭配使用者定義的比較和算術運算子？</span><span class="sxs-lookup"><span data-stu-id="870d4-138">**Open Issue:** should we allow usages of a target-typed `new` with user-defined comparison and arithmetic operators?</span></span>

<span data-ttu-id="870d4-139">為了進行比較，`default` 只支援相等（使用者定義和內建）運算子。</span><span class="sxs-lookup"><span data-stu-id="870d4-139">For comparison, `default` only supports equality (user-defined and built-in) operators.</span></span> <span data-ttu-id="870d4-140">同時支援 `new()` 的其他運算子也是有意義的嗎？</span><span class="sxs-lookup"><span data-stu-id="870d4-140">Would it make sense to support other operators for `new()` as well?</span></span>

## <a name="drawbacks"></a><span data-ttu-id="870d4-141">缺點</span><span class="sxs-lookup"><span data-stu-id="870d4-141">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="870d4-142">無。</span><span class="sxs-lookup"><span data-stu-id="870d4-142">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="870d4-143">替代方案</span><span class="sxs-lookup"><span data-stu-id="870d4-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="870d4-144">大部分關於類型在欄位初始化中無法重複的抱怨，都是關於類型*引數*，而不是類型本身，我們可以只推斷類型引數，例如 `new Dictionary(...)` （或類似），並從引數或集合初始化運算式在本機推斷類型引數。</span><span class="sxs-lookup"><span data-stu-id="870d4-144">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="870d4-145">問題</span><span class="sxs-lookup"><span data-stu-id="870d4-145">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="870d4-146">是否應該禁止運算式樹狀架構中的使用方式？</span><span class="sxs-lookup"><span data-stu-id="870d4-146">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="870d4-147">不要</span><span class="sxs-lookup"><span data-stu-id="870d4-147">(no)</span></span>
- <span data-ttu-id="870d4-148">功能如何與 `dynamic` 引數互動？</span><span class="sxs-lookup"><span data-stu-id="870d4-148">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="870d4-149">（無特殊處理）</span><span class="sxs-lookup"><span data-stu-id="870d4-149">(no special treatment)</span></span>
- <span data-ttu-id="870d4-150">IntelliSense 應該如何與 `new()`搭配使用？</span><span class="sxs-lookup"><span data-stu-id="870d4-150">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="870d4-151">（只有在只有單一目標型別時）</span><span class="sxs-lookup"><span data-stu-id="870d4-151">(only when there is a single target-type)</span></span>
## <a name="design-meetings"></a><span data-ttu-id="870d4-152">設計會議</span><span class="sxs-lookup"><span data-stu-id="870d4-152">Design meetings</span></span>

- [<span data-ttu-id="870d4-153">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="870d4-153">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
