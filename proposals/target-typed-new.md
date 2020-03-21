---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108362"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="0693d-101">目標型別 `new` 運算式</span><span class="sxs-lookup"><span data-stu-id="0693d-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="0693d-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="0693d-102">[x] Proposed</span></span>
* <span data-ttu-id="0693d-103">[x][原型](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="0693d-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="0693d-104">[] 執行</span><span class="sxs-lookup"><span data-stu-id="0693d-104">[ ] Implementation</span></span>
* <span data-ttu-id="0693d-105">[] 規格</span><span class="sxs-lookup"><span data-stu-id="0693d-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="0693d-106">摘要</span><span class="sxs-lookup"><span data-stu-id="0693d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="0693d-107">當已知型別時，不需要型別規格。</span><span class="sxs-lookup"><span data-stu-id="0693d-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="0693d-108">動機</span><span class="sxs-lookup"><span data-stu-id="0693d-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="0693d-109">允許欄位初始化，而不復制類型。</span><span class="sxs-lookup"><span data-stu-id="0693d-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="0693d-110">當可以從使用方式推斷時，允許省略類型。</span><span class="sxs-lookup"><span data-stu-id="0693d-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="0693d-111">將物件具現化，而不將型別拼出。</span><span class="sxs-lookup"><span data-stu-id="0693d-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="0693d-112">詳細設計</span><span class="sxs-lookup"><span data-stu-id="0693d-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="0693d-113">當有括弧時，會修改*object_creation_expression*語法，讓*類型*成為選擇性。</span><span class="sxs-lookup"><span data-stu-id="0693d-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="0693d-114">這是解決*anonymous_object_creation_expression*不明確的必要工作。</span><span class="sxs-lookup"><span data-stu-id="0693d-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="0693d-115">目標具類型的 `new` 可以轉換成任何類型。</span><span class="sxs-lookup"><span data-stu-id="0693d-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="0693d-116">因此，它不會影響多載解析。</span><span class="sxs-lookup"><span data-stu-id="0693d-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="0693d-117">這主要是為了避免無法預期的重大變更。</span><span class="sxs-lookup"><span data-stu-id="0693d-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="0693d-118">在決定型別之後，將會系結引數清單和初始化運算式運算式。</span><span class="sxs-lookup"><span data-stu-id="0693d-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="0693d-119">運算式的型別是從目標型別推斷而來，必須是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="0693d-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="0693d-120">**任何結構類型**（包括元組類型）</span><span class="sxs-lookup"><span data-stu-id="0693d-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="0693d-121">**任何參考型別**（包括委派類型）</span><span class="sxs-lookup"><span data-stu-id="0693d-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="0693d-122">具有函數或 `struct` 條件約束的**任何類型參數**</span><span class="sxs-lookup"><span data-stu-id="0693d-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="0693d-123">但有下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="0693d-123">with the following exceptions:</span></span>

- <span data-ttu-id="0693d-124">**列舉類型：** 並非所有列舉類型都包含常數零，因此最好使用明確列舉成員。</span><span class="sxs-lookup"><span data-stu-id="0693d-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="0693d-125">**介面類別型：** 這是一項小規模功能，最好是明確提及該類型。</span><span class="sxs-lookup"><span data-stu-id="0693d-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="0693d-126">**陣列類型：** 陣列需要特殊的語法來提供長度。</span><span class="sxs-lookup"><span data-stu-id="0693d-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="0693d-127">**動態：** 我們不允許 `new dynamic()`，因此不允許以 `dynamic` 作為目標型別的 `new()`。</span><span class="sxs-lookup"><span data-stu-id="0693d-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="0693d-128">也會排除*object_creation_expression*中不允許的所有其他類型，例如指標類型。</span><span class="sxs-lookup"><span data-stu-id="0693d-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="0693d-129">當目標型別是可為 null 的實值型別時，目標型別 `new` 將會轉換成基礎型別，而不是可為 null 的型別。</span><span class="sxs-lookup"><span data-stu-id="0693d-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="0693d-130">**開啟問題：** 我們是否允許委派和元組作為目標型別？</span><span class="sxs-lookup"><span data-stu-id="0693d-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="0693d-131">上述規則包括委派（參考型別）和元組（結構型別）。</span><span class="sxs-lookup"><span data-stu-id="0693d-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="0693d-132">雖然這兩種類型都是可建構的，但如果類型是 inferable，則可以使用匿名函式或元組常值。</span><span class="sxs-lookup"><span data-stu-id="0693d-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
