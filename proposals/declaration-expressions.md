---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484577"
---
# <a name="declaration-expressions"></a><span data-ttu-id="45a24-101">宣告運算式</span><span class="sxs-lookup"><span data-stu-id="45a24-101">Declaration expressions</span></span>

<span data-ttu-id="45a24-102">支援將宣告指派為運算式。</span><span class="sxs-lookup"><span data-stu-id="45a24-102">Support declaration assignments as expressions.</span></span>

## <a name="motivation"></a><span data-ttu-id="45a24-103">動機</span><span class="sxs-lookup"><span data-stu-id="45a24-103">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="45a24-104">允許在更多情況下于宣告時進行初始化、簡化程式碼，以及允許使用 `var`。</span><span class="sxs-lookup"><span data-stu-id="45a24-104">Allow initialization at the point of declaration in more cases, simplifying code, and allowing `var` to be used.</span></span>

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

<span data-ttu-id="45a24-105">允許 `ref` 引數的宣告，類似于 `out var`。</span><span class="sxs-lookup"><span data-stu-id="45a24-105">Allow declarations for `ref` arguments, similar to `out var`.</span></span>

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a><span data-ttu-id="45a24-106">詳細設計</span><span class="sxs-lookup"><span data-stu-id="45a24-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="45a24-107">運算式已擴充為包含宣告指派。</span><span class="sxs-lookup"><span data-stu-id="45a24-107">Expressions are extended to include declaration assignment.</span></span> <span data-ttu-id="45a24-108">優先順序與指派相同。</span><span class="sxs-lookup"><span data-stu-id="45a24-108">Precedence is the same as assignment.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

<span data-ttu-id="45a24-109">宣告指派是單一的本機。</span><span class="sxs-lookup"><span data-stu-id="45a24-109">The declaration assignment is of a single local.</span></span>

<span data-ttu-id="45a24-110">宣告指派運算式的類型是宣告的類型。</span><span class="sxs-lookup"><span data-stu-id="45a24-110">The type of a declaration assignment expression is the type of the declaration.</span></span>
<span data-ttu-id="45a24-111">如果類型是 `var`，則推斷的類型是初始化運算式的類型。</span><span class="sxs-lookup"><span data-stu-id="45a24-111">If the type is `var`, the inferred type is the type of the initializing expression.</span></span> 

<span data-ttu-id="45a24-112">宣告指派運算式可以是左值，特別是 `ref` 的引數值。</span><span class="sxs-lookup"><span data-stu-id="45a24-112">The declaration assignment expression may be an l-value, for `ref` argument values in particular.</span></span>

<span data-ttu-id="45a24-113">如果宣告指派運算式宣告實值型別，而且運算式是右值，則運算式的值會是複本。</span><span class="sxs-lookup"><span data-stu-id="45a24-113">If the declaration assignment expression declares a value type, and the expression is an r-value, the value of the expression is a copy.</span></span>

<span data-ttu-id="45a24-114">宣告指派運算式可以宣告 `ref` 本機。</span><span class="sxs-lookup"><span data-stu-id="45a24-114">The declaration assignment expression may declare a `ref` local.</span></span>
<span data-ttu-id="45a24-115">當 `ref` 用於 `ref` 引數中的宣告運算式時，會有一個不明確的情況。</span><span class="sxs-lookup"><span data-stu-id="45a24-115">There is an ambiguity when `ref` is used for a declaration expression in a `ref` argument.</span></span>
<span data-ttu-id="45a24-116">本機變數初始化運算式會判斷宣告是否為 `ref` 本機。</span><span class="sxs-lookup"><span data-stu-id="45a24-116">The local variable initializer determines whether the declaration is a `ref` local.</span></span>

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

<span data-ttu-id="45a24-117">宣告指派運算式中宣告的區域變數範圍，與 C # 7.0 中對應宣告運算式的範圍相同。</span><span class="sxs-lookup"><span data-stu-id="45a24-117">The scope of locals declared in declaration assignment expressions is the same the scope of corresponding declaration expressions from C#7.0.</span></span>

<span data-ttu-id="45a24-118">在宣告運算式前面的文字中參考本機，是編譯時間錯誤。</span><span class="sxs-lookup"><span data-stu-id="45a24-118">It is a compile time error to refer to a local in text preceding the declaration expression.</span></span>

## <a name="alternatives"></a><span data-ttu-id="45a24-119">替代方案</span><span class="sxs-lookup"><span data-stu-id="45a24-119">Alternatives</span></span>
[alternatives]: #alternatives
<span data-ttu-id="45a24-120">無變更。</span><span class="sxs-lookup"><span data-stu-id="45a24-120">No change.</span></span> <span data-ttu-id="45a24-121">這項功能只是語法上的縮寫。</span><span class="sxs-lookup"><span data-stu-id="45a24-121">This feature is just syntactic shorthand after all.</span></span>

<span data-ttu-id="45a24-122">更多一般序列運算式：請參閱[#377](https://github.com/dotnet/csharplang/issues/377)。</span><span class="sxs-lookup"><span data-stu-id="45a24-122">More general sequence expressions: see [#377](https://github.com/dotnet/csharplang/issues/377).</span></span>

<span data-ttu-id="45a24-123">若要允許在更多情況下使用 `var`，請允許 `var` 區域變數的個別宣告和指派，並從所有程式碼路徑的指派推斷類型。</span><span class="sxs-lookup"><span data-stu-id="45a24-123">To allow use of `var` in more cases, allow separate declaration and assignment of `var` locals, and infer the type from assignments from all code paths.</span></span>

## <a name="see-also"></a><span data-ttu-id="45a24-124">另請參閱</span><span class="sxs-lookup"><span data-stu-id="45a24-124">See also</span></span>
[see-also]: #see-also
<span data-ttu-id="45a24-125">請參閱[#595](https://github.com/dotnet/csharplang/issues/595)中的基本宣告運算式。</span><span class="sxs-lookup"><span data-stu-id="45a24-125">See Basic Declaration Expression in [#595](https://github.com/dotnet/csharplang/issues/595).</span></span>

<span data-ttu-id="45a24-126">請參閱[解構](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md)功能中的解構宣告。</span><span class="sxs-lookup"><span data-stu-id="45a24-126">See Deconstruction Declaration in the [deconstruction](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) feature.</span></span>
