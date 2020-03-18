---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2020
ms.locfileid: "79485207"
---
# <a name="nullable-reference-types-specification"></a><span data-ttu-id="39a72-101">可為 null 的參考型別規格</span><span class="sxs-lookup"><span data-stu-id="39a72-101">Nullable Reference Types Specification</span></span>

<span data-ttu-id="39a72-102">這是進行中的工作-有幾個部分遺失或不完整。</span><span class="sxs-lookup"><span data-stu-id="39a72-102">\*\*\* This is a work in progress - several parts are missing or incomplete.</span></span> ***

## <a name="syntax"></a><span data-ttu-id="39a72-103">語法</span><span class="sxs-lookup"><span data-stu-id="39a72-103">Syntax</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="39a72-104">可為 Null 的參考型別</span><span class="sxs-lookup"><span data-stu-id="39a72-104">Nullable reference types</span></span>

<span data-ttu-id="39a72-105">可為 null 的參考型別與簡短形式的可為 null 實數值型別具有相同的語法 `T?`，但沒有對應的長格式。</span><span class="sxs-lookup"><span data-stu-id="39a72-105">Nullable reference types have the same syntax `T?` as the short form of nullable value types, but do not have a corresponding long form.</span></span>

<span data-ttu-id="39a72-106">基於規格的目的，目前的 `nullable_type` 生產會重新命名為 `nullable_value_type`，並新增 `nullable_reference_type` 的生產環境：</span><span class="sxs-lookup"><span data-stu-id="39a72-106">For the purposes of the specification, the current `nullable_type` production is renamed to `nullable_value_type`, and a `nullable_reference_type` production is added:</span></span>

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

<span data-ttu-id="39a72-107">`nullable_reference_type` 中的 `non_nullable_reference_type` 必須是不可為 null 的參考型別（類別、介面、委派或陣列），或是限制為不可為 null 的參考型別的類型參數（透過 `class` 條件約束，或 `object`以外的類別）。</span><span class="sxs-lookup"><span data-stu-id="39a72-107">The `non_nullable_reference_type` in a `nullable_reference_type` must be a non-nullable reference type (class, interface, delegate or array), or a type parameter that is constrained to be a non-nullable reference type (through the `class` constraint, or a class other than `object`).</span></span>

<span data-ttu-id="39a72-108">可為 null 的參考型別不能出現在下列位置：</span><span class="sxs-lookup"><span data-stu-id="39a72-108">Nullable reference types cannot occur in the following positions:</span></span>

- <span data-ttu-id="39a72-109">做為基類或介面</span><span class="sxs-lookup"><span data-stu-id="39a72-109">as a base class or interface</span></span>
- <span data-ttu-id="39a72-110">做為 `member_access` 的接收者</span><span class="sxs-lookup"><span data-stu-id="39a72-110">as the receiver of a `member_access`</span></span>
- <span data-ttu-id="39a72-111">做為 `object_creation_expression` 中的 `type`</span><span class="sxs-lookup"><span data-stu-id="39a72-111">as the `type` in an `object_creation_expression`</span></span>
- <span data-ttu-id="39a72-112">做為 `delegate_creation_expression` 中的 `delegate_type`</span><span class="sxs-lookup"><span data-stu-id="39a72-112">as the `delegate_type` in a `delegate_creation_expression`</span></span>
- <span data-ttu-id="39a72-113">做為 `is_expression`中的 `type`、`catch_clause` 或 `type_pattern`</span><span class="sxs-lookup"><span data-stu-id="39a72-113">as the `type` in an `is_expression`, a `catch_clause` or a `type_pattern`</span></span>
- <span data-ttu-id="39a72-114">作為完整介面成員名稱中的 `interface`</span><span class="sxs-lookup"><span data-stu-id="39a72-114">as the `interface` in a fully qualified interface member name</span></span>

<span data-ttu-id="39a72-115">警告是在可為 null 注釋內容已停用的 `nullable_reference_type` 上提供。</span><span class="sxs-lookup"><span data-stu-id="39a72-115">A warning is given on a `nullable_reference_type` where the nullable annotation context is disabled.</span></span>

### <a name="nullable-class-constraint"></a><span data-ttu-id="39a72-116">可為 null 的類別條件約束</span><span class="sxs-lookup"><span data-stu-id="39a72-116">Nullable class constraint</span></span>

<span data-ttu-id="39a72-117">`class` 條件約束具有可為 null 的對應 `class?`：</span><span class="sxs-lookup"><span data-stu-id="39a72-117">The `class` constraint has a nullable counterpart `class?`:</span></span>

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a><span data-ttu-id="39a72-118">Null-容許運算子</span><span class="sxs-lookup"><span data-stu-id="39a72-118">The null-forgiving operator</span></span>

<span data-ttu-id="39a72-119">修正後 `!` 運算子稱為 null 容許運算子。</span><span class="sxs-lookup"><span data-stu-id="39a72-119">The post-fix `!` operator is called the null-forgiving operator.</span></span>

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

<span data-ttu-id="39a72-120">`primary_expression` 必須是參考型別。</span><span class="sxs-lookup"><span data-stu-id="39a72-120">The `primary_expression` must be of a reference type.</span></span>  

<span data-ttu-id="39a72-121">後置 `!` 運算子沒有執行時間效果-它會評估為基礎運算式的結果。</span><span class="sxs-lookup"><span data-stu-id="39a72-121">The postfix `!` operator has no runtime effect - it evaluates to the result of the underlying expression.</span></span> <span data-ttu-id="39a72-122">其唯一的角色是變更運算式的 null 狀態，並限制其使用所提供的警告。</span><span class="sxs-lookup"><span data-stu-id="39a72-122">Its only role is to change the null state of the expression, and to limit warnings given on its use.</span></span>

### <a name="nullable-implicitly-typed-local-variables"></a><span data-ttu-id="39a72-123">可為 null 的隱含類型區域變數</span><span class="sxs-lookup"><span data-stu-id="39a72-123">nullable implicitly typed local variables</span></span>

<span data-ttu-id="39a72-124">`var` 推斷參考型別的批註類型。</span><span class="sxs-lookup"><span data-stu-id="39a72-124">`var` infers an annotated type for reference types.</span></span>
<span data-ttu-id="39a72-125">例如，在 `var s = "";` 會將 `var` 推斷為 `string?`。</span><span class="sxs-lookup"><span data-stu-id="39a72-125">For instance, in `var s = "";` the `var` is inferred as `string?`.</span></span>

### <a name="nullable-compiler-directives"></a><span data-ttu-id="39a72-126">可為 null 的編譯器指示詞</span><span class="sxs-lookup"><span data-stu-id="39a72-126">Nullable compiler directives</span></span>

<span data-ttu-id="39a72-127">`#nullable` 指示詞會控制可為 null 的注釋和警告內容。</span><span class="sxs-lookup"><span data-stu-id="39a72-127">`#nullable` directives control the nullable annotation and warning contexts.</span></span>

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

<span data-ttu-id="39a72-128">`#pragma warning` 指示詞會展開以允許變更可為 null 的警告內容，並允許在上啟用個別警告，即使它們預設為停用也一樣：</span><span class="sxs-lookup"><span data-stu-id="39a72-128">`#pragma warning` directives are expanded to allow changing the nullable warning context, and to allow individual warnings to be enabled on even when they're disabled by default:</span></span>

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

<span data-ttu-id="39a72-129">請注意，新形式的 `pragma_warning_body` 會使用 `nullable_action`，而不是 `warning_action`。</span><span class="sxs-lookup"><span data-stu-id="39a72-129">Note that the new form of `pragma_warning_body` uses `nullable_action`, not `warning_action`.</span></span>

## <a name="nullable-contexts"></a><span data-ttu-id="39a72-130">可為 Null 內容</span><span class="sxs-lookup"><span data-stu-id="39a72-130">Nullable contexts</span></span>

<span data-ttu-id="39a72-131">原始程式碼的每一行都有*可為 null 的注釋內容*和*可為 null 的警告內容*。</span><span class="sxs-lookup"><span data-stu-id="39a72-131">Every line of source code has a *nullable annotation context* and a *nullable warning context*.</span></span> <span data-ttu-id="39a72-132">這些控制項會控制可為 null 的注釋是否有效果，以及是否提供 null 屬性的警告。</span><span class="sxs-lookup"><span data-stu-id="39a72-132">These control whether nullable annotations have effect, and whether nullability warnings are given.</span></span> <span data-ttu-id="39a72-133">指定行的注釋內容為*停用*或*啟用*。</span><span class="sxs-lookup"><span data-stu-id="39a72-133">The annotation context of a given line is either *disabled* or *enabled*.</span></span> <span data-ttu-id="39a72-134">指定行的警告內容已*停用*或*啟用*。</span><span class="sxs-lookup"><span data-stu-id="39a72-134">The warning context of a given line is either *disabled* or *enabled*.</span></span>

<span data-ttu-id="39a72-135">這兩個內容可以在專案層級（在C#原始程式碼外部）或原始程式檔內的任何位置，透過 `#nullable` 的預處理器指示詞來指定。</span><span class="sxs-lookup"><span data-stu-id="39a72-135">Both contexts can be specified at the project level (outside of C# source code), or anywhere within a source file via `#nullable` pre-processor directives.</span></span> <span data-ttu-id="39a72-136">如果未提供專案層級設定，則預設值為*停用*這兩個內容。</span><span class="sxs-lookup"><span data-stu-id="39a72-136">If no project level settings are provided the default is for both contexts to be *disabled*.</span></span>

<span data-ttu-id="39a72-137">`#nullable` 指示詞會控制來源文字中的注釋和警告內容，並優先于專案層級的設定。</span><span class="sxs-lookup"><span data-stu-id="39a72-137">The `#nullable` directive controls the annotation and warning contexts within the source text, and take precedence over the project-level settings.</span></span>

<span data-ttu-id="39a72-138">指示詞會設定 it 針對後續程式程式碼所控制的內容，直到另一個指示詞覆寫它，或直到原始程式檔結束為止。</span><span class="sxs-lookup"><span data-stu-id="39a72-138">A directive sets the context(s) it controls for subsequent lines of code, until another directive overrides it, or until the end of the source file.</span></span>

<span data-ttu-id="39a72-139">指示詞的效果如下所示：</span><span class="sxs-lookup"><span data-stu-id="39a72-139">The effect of the directives is as follows:</span></span>

- <span data-ttu-id="39a72-140">`#nullable disable`：將可為 null 注釋和警告內容設定為*停用*</span><span class="sxs-lookup"><span data-stu-id="39a72-140">`#nullable disable`: Sets the nullable annotation and warning contexts to *disabled*</span></span>
- <span data-ttu-id="39a72-141">`#nullable enable`：將可為 null 注釋和警告內容設定為*已啟用*</span><span class="sxs-lookup"><span data-stu-id="39a72-141">`#nullable enable`: Sets the nullable annotation and warning contexts to *enabled*</span></span>
- <span data-ttu-id="39a72-142">`#nullable restore`：將可為 null 的注釋和警告內容還原至專案設定</span><span class="sxs-lookup"><span data-stu-id="39a72-142">`#nullable restore`: Restores the nullable annotation and warning contexts to project settings</span></span>
- <span data-ttu-id="39a72-143">`#nullable disable annotations`：將可為 null 注釋內容設定為*停用*</span><span class="sxs-lookup"><span data-stu-id="39a72-143">`#nullable disable annotations`: Sets the nullable annotation context to *disabled*</span></span>
- <span data-ttu-id="39a72-144">`#nullable enable annotations`：將可為 null 注釋內容設定為*已啟用*</span><span class="sxs-lookup"><span data-stu-id="39a72-144">`#nullable enable annotations`: Sets the nullable annotation context to *enabled*</span></span>
- <span data-ttu-id="39a72-145">`#nullable restore annotations`：將可為 null 注釋內容還原至專案設定</span><span class="sxs-lookup"><span data-stu-id="39a72-145">`#nullable restore annotations`: Restores the nullable annotation context to project settings</span></span>
- <span data-ttu-id="39a72-146">`#nullable disable warnings`：將可為 null 警告內容設定為*停用*</span><span class="sxs-lookup"><span data-stu-id="39a72-146">`#nullable disable warnings`: Sets the nullable warning context to *disabled*</span></span>
- <span data-ttu-id="39a72-147">`#nullable enable warnings`：將可為 null 警告內容設定為*已啟用*</span><span class="sxs-lookup"><span data-stu-id="39a72-147">`#nullable enable warnings`: Sets the nullable warning context to *enabled*</span></span>
- <span data-ttu-id="39a72-148">`#nullable restore warnings`：將可為 null 警告內容還原至專案設定</span><span class="sxs-lookup"><span data-stu-id="39a72-148">`#nullable restore warnings`: Restores the nullable warning context to project settings</span></span>

## <a name="nullability-of-types"></a><span data-ttu-id="39a72-149">型別的可 NULL 性</span><span class="sxs-lookup"><span data-stu-id="39a72-149">Nullability of types</span></span>

<span data-ttu-id="39a72-150">給定的類型可以有四個 nullabilities 的其中一個：*遺忘式*、*不可為 null*、 *nullable*和*unknown*。</span><span class="sxs-lookup"><span data-stu-id="39a72-150">A given type can have one of four nullabilities: *Oblivious*, *nonnullable*, *nullable* and *unknown*.</span></span> 

<span data-ttu-id="39a72-151">*不可為 null*和*未知*的類型可能會在指派可能的 `null` 值時造成警告。</span><span class="sxs-lookup"><span data-stu-id="39a72-151">*Nonnullable* and *unknown* types may cause warnings if a potential `null` value is assigned to them.</span></span> <span data-ttu-id="39a72-152">不過，*遺忘式*和*nullable*型別都是「可指派*null*」，而且可以在沒有警告的情況下指派 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="39a72-152">*Oblivious* and *nullable* types, however, are "*null-assignable*" and can have `null` values assigned to them without warnings.</span></span> 

<span data-ttu-id="39a72-153">*遺忘式*和*不可為 null*類型可以解除引用或指派，而不會出現警告。</span><span class="sxs-lookup"><span data-stu-id="39a72-153">*Oblivious* and *nonnullable* types can be dereferenced or assigned without warnings.</span></span> <span data-ttu-id="39a72-154">不過， *nullable*和*unknown*類型的值都是「*null*」，而且可能會在解除引用或指派時造成警告，而不會有適當的 null 檢查。</span><span class="sxs-lookup"><span data-stu-id="39a72-154">Values of *nullable* and *unknown* types, however, are "*null-yielding*" and may cause warnings when dereferenced or assigned without proper null checking.</span></span> 

<span data-ttu-id="39a72-155">Null 產生類型的*預設 null 狀態*為「可能是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-155">The *default null state* of a null-yielding type is "maybe null".</span></span> <span data-ttu-id="39a72-156">非 null 產生類型的預設 null 狀態為「非 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-156">The default null state of a non-null-yielding type is "not null".</span></span>

<span data-ttu-id="39a72-157">類型和在判斷其 null 屬性中所發生的可為 null 注釋內容的種類：</span><span class="sxs-lookup"><span data-stu-id="39a72-157">The kind of type and the nullable annotation context it occurs in determine its nullability:</span></span>

- <span data-ttu-id="39a72-158">不可為 null 數值型別 `S` 一律為*不可為 null*</span><span class="sxs-lookup"><span data-stu-id="39a72-158">A nonnullable value type `S` is always *nonnullable*</span></span>
- <span data-ttu-id="39a72-159">可為 null 的實數值型別 `S?` 一律*可為 null*</span><span class="sxs-lookup"><span data-stu-id="39a72-159">A nullable value type `S?` is always *nullable*</span></span>
- <span data-ttu-id="39a72-160">已*停用*注釋內容中 `C` 的 unannotated 參考型別為*遺忘式*</span><span class="sxs-lookup"><span data-stu-id="39a72-160">An unannotated reference type `C` in a *disabled* annotation context is *oblivious*</span></span>
- <span data-ttu-id="39a72-161">*已啟用*注釋內容中 `C` 的 unannotated 參考型別為*不可為 null*</span><span class="sxs-lookup"><span data-stu-id="39a72-161">An unannotated reference type `C` in an *enabled* annotation context is *nonnullable*</span></span>
- <span data-ttu-id="39a72-162">可為 null 的參考型別 `C?` 可為*null* （但可能會在*停用*的注釋內容中產生警告）</span><span class="sxs-lookup"><span data-stu-id="39a72-162">A nullable reference type `C?` is *nullable* (but a warning may be yielded in a *disabled* annotation context)</span></span>

<span data-ttu-id="39a72-163">類型參數會另外將其條件約束納入考慮：</span><span class="sxs-lookup"><span data-stu-id="39a72-163">Type parameters additionally take their constraints into account:</span></span>

- <span data-ttu-id="39a72-164">類型參數 `T`，其中所有條件約束（如果有的話）為 null 產生類型（*可為 null*和*未知*），或 `class?` 條件約束*不明*</span><span class="sxs-lookup"><span data-stu-id="39a72-164">A type parameter `T` where all constraints (if any) are either null-yielding types (*nullable* and *unknown*) or the `class?` constraint is *unknown*</span></span>
- <span data-ttu-id="39a72-165">類型參數 `T`，其中至少有一個條件約束為*遺忘式*或*不可為 null* ，或其中一個 `struct` 或 `class` 條件約束為</span><span class="sxs-lookup"><span data-stu-id="39a72-165">A type parameter `T` where at least one constraint is either *oblivious* or *nonnullable* or one of the `struct` or `class` constraints is</span></span>
    - <span data-ttu-id="39a72-166">*已停*用注釋內容中的*遺忘式*</span><span class="sxs-lookup"><span data-stu-id="39a72-166">*oblivious* in a *disabled* annotation context</span></span>
    - <span data-ttu-id="39a72-167">*已啟用*注釋內容中的*不可為 null*</span><span class="sxs-lookup"><span data-stu-id="39a72-167">*nonnullable* in an *enabled* annotation context</span></span>
- <span data-ttu-id="39a72-168">可為 null 的型別參數 `T?`，其中至少有一個 `T`的條件約束為*遺忘式*或*不可為 null* ，或其中一個 `struct` 或 `class` 條件約束為</span><span class="sxs-lookup"><span data-stu-id="39a72-168">A nullable type parameter `T?` where at least one of `T`'s constraints is *oblivious* or *nonnullable* or one of the `struct` or `class` constraints, is</span></span>
    - <span data-ttu-id="39a72-169">*停*用的注釋內容中*為 nullable* （但產生警告）</span><span class="sxs-lookup"><span data-stu-id="39a72-169">*nullable* in a *disabled* annotation context (but a warning is yielded)</span></span>
    - <span data-ttu-id="39a72-170">*啟用*的注釋內容中*可為 null*</span><span class="sxs-lookup"><span data-stu-id="39a72-170">*nullable* in an *enabled* annotation context</span></span>

<span data-ttu-id="39a72-171">`T`的型別參數，只有在 `T` 已知為實值型別或已知為引用型別時，才允許 `T?`。</span><span class="sxs-lookup"><span data-stu-id="39a72-171">For a type parameter `T`, `T?` is only allowed if `T` is known to be a value type or known to be a reference type.</span></span>

### <a name="oblivious-vs-nonnullable"></a><span data-ttu-id="39a72-172">遺忘式與不可為 null</span><span class="sxs-lookup"><span data-stu-id="39a72-172">Oblivious vs nonnullable</span></span>

<span data-ttu-id="39a72-173">當類型的最後一個 token 在該內容中時，會將 `type` 視為在指定的注釋內容中發生。</span><span class="sxs-lookup"><span data-stu-id="39a72-173">A `type` is deemed to occur in a given annotation context when the last token of the type is within that context.</span></span>

<span data-ttu-id="39a72-174">在原始程式碼中 `C` 指定的參考型別是否會解讀為遺忘式或不可為 null，取決於該原始程式碼的注釋內容。</span><span class="sxs-lookup"><span data-stu-id="39a72-174">Whether a given reference type `C` in source code is interpreted as oblivious or nonnullable depends on the annotation context of that source code.</span></span> <span data-ttu-id="39a72-175">但是一旦建立之後，它就會被視為該類型的一部分，並在替代泛型型別引數時，使用它來傳送。</span><span class="sxs-lookup"><span data-stu-id="39a72-175">But once established, it is considered part of that type, and "travels with it" e.g. during substitution of generic type arguments.</span></span> <span data-ttu-id="39a72-176">就像在型別上有 `?` 的注釋一樣，但不會出現。</span><span class="sxs-lookup"><span data-stu-id="39a72-176">It is as if there is an annotation like `?` on the type, but invisible.</span></span>

## <a name="constraints"></a><span data-ttu-id="39a72-177">條件約束</span><span class="sxs-lookup"><span data-stu-id="39a72-177">Constraints</span></span>

<span data-ttu-id="39a72-178">可為 null 的參考型別可以當做泛型條件約束使用。</span><span class="sxs-lookup"><span data-stu-id="39a72-178">Nullable reference types can be used as generic constraints.</span></span> <span data-ttu-id="39a72-179">此外 `object` 現在也有效，做為明確的條件約束。</span><span class="sxs-lookup"><span data-stu-id="39a72-179">Furthermore `object` is now valid as an explicit constraint.</span></span> <span data-ttu-id="39a72-180">缺少條件約束現在等同于 `object?` 條件約束（而不是 `object`），但（不同于之前的 `object`） `object?` 不會被禁止為明確條件約束。</span><span class="sxs-lookup"><span data-stu-id="39a72-180">Absence of a constraint is now equivalent to an `object?` constraint (instead of `object`), but (unlike `object` before) `object?` is not prohibited as an explicit constraint.</span></span>

<span data-ttu-id="39a72-181">`class?` 是表示「可能可為 null 的參考型別」的新條件約束，而 `class` 則代表 "不可為 null reference type"。</span><span class="sxs-lookup"><span data-stu-id="39a72-181">`class?` is a new constraint denoting "possibly nullable reference type", whereas `class` denotes "nonnullable reference type".</span></span>

<span data-ttu-id="39a72-182">型別引數或條件約束的 null 屬性不會影響型別是否符合條件約束，但目前的情況（可為 null 的實值型別不符合 `struct` 條件約束）除外。</span><span class="sxs-lookup"><span data-stu-id="39a72-182">The nullability of a type argument or of a constraint does not impact whether the type satisfies the constraint, except where that is already the case today (nullable value types do not satisfy the `struct` constraint).</span></span> <span data-ttu-id="39a72-183">不過，如果類型引數不符合條件約束的 null 屬性需求，可能會提供警告。</span><span class="sxs-lookup"><span data-stu-id="39a72-183">However, if the type argument does not satisfy the nullability requirements of the constraint, a warning may be given.</span></span>

## <a name="null-state-and-null-tracking"></a><span data-ttu-id="39a72-184">Null 狀態和 null 追蹤</span><span class="sxs-lookup"><span data-stu-id="39a72-184">Null state and null tracking</span></span>

<span data-ttu-id="39a72-185">指定來源位置中的每個運算式都有*null 狀態*，指出是否認為它可能會評估為 null。</span><span class="sxs-lookup"><span data-stu-id="39a72-185">Every expression in a given source location has a *null state*, which indicated whether it is believed to potentially evaluate to null.</span></span> <span data-ttu-id="39a72-186">Null 狀態是「不是 null」或「可能是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-186">The null state is either "not null" or "maybe null".</span></span> <span data-ttu-id="39a72-187">Null 狀態是用來判斷是否應該提供有關 null 不安全轉換和取值的警告。</span><span class="sxs-lookup"><span data-stu-id="39a72-187">The null state is used to determine whether a warning should be given about null-unsafe conversions and dereferences.</span></span>

### <a name="null-tracking-for-variables"></a><span data-ttu-id="39a72-188">變數的 Null 追蹤</span><span class="sxs-lookup"><span data-stu-id="39a72-188">Null tracking for variables</span></span>

<span data-ttu-id="39a72-189">對於表示變數或屬性的特定運算式，會根據指派的專案，在發生次數之間追蹤 null 狀態，並在其上執行測試，並在兩者之間進行控制流程。</span><span class="sxs-lookup"><span data-stu-id="39a72-189">For certain expressions denoting variables or properties, the null state is tracked between occurrences, based on assignments to them, tests performed on them and the control flow between them.</span></span> <span data-ttu-id="39a72-190">這類似于針對變數追蹤明確指派的方式。</span><span class="sxs-lookup"><span data-stu-id="39a72-190">This is similar to how definite assignment is tracked for variables.</span></span> <span data-ttu-id="39a72-191">追蹤的運算式是下列形式的其中一種：</span><span class="sxs-lookup"><span data-stu-id="39a72-191">The tracked expressions are the ones of the following form:</span></span>

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

<span data-ttu-id="39a72-192">其中識別碼代表欄位或屬性。</span><span class="sxs-lookup"><span data-stu-id="39a72-192">Where the identifiers denote fields or properties.</span></span>

<span data-ttu-id="39a72-193">***描述 null 狀態轉換，類似于明確指派***</span><span class="sxs-lookup"><span data-stu-id="39a72-193">***Describe null state transitions similar to definite assignment***</span></span>

### <a name="null-state-for-expressions"></a><span data-ttu-id="39a72-194">運算式的 Null 狀態</span><span class="sxs-lookup"><span data-stu-id="39a72-194">Null state for expressions</span></span>

<span data-ttu-id="39a72-195">運算式的 null 狀態衍生自其表單和類型，以及其相關變數的 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-195">The null state of an expression is derived from its form and type, and from the null state of variables involved in it.</span></span>

### <a name="literals"></a><span data-ttu-id="39a72-196">常值</span><span class="sxs-lookup"><span data-stu-id="39a72-196">Literals</span></span>

<span data-ttu-id="39a72-197">`null` 常值的 null 狀態為「可能是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-197">The null state of a `null` literal is "maybe null".</span></span> <span data-ttu-id="39a72-198">要轉換成已知不是不可為 null 數值型別之類型的 `default` 常值的 null 狀態是「可能是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-198">The null state of a `default` literal that is being converted to a type that is known not to be a nonnullable value type is "maybe null".</span></span> <span data-ttu-id="39a72-199">任何其他常值的 null 狀態為「不是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-199">The null state of any other literal is "not null".</span></span>

### <a name="simple-names"></a><span data-ttu-id="39a72-200">簡單名稱</span><span class="sxs-lookup"><span data-stu-id="39a72-200">Simple names</span></span>

<span data-ttu-id="39a72-201">如果 `simple_name` 未分類為值，其 null 狀態會是「不是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-201">If a `simple_name` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="39a72-202">否則，它是追蹤的運算式，而其 null 狀態是其在此來源位置的追蹤 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-202">Otherwise it is a tracked expression, and its null state is its tracked null state at this source location.</span></span>

### <a name="member-access"></a><span data-ttu-id="39a72-203">成員存取</span><span class="sxs-lookup"><span data-stu-id="39a72-203">Member access</span></span>

<span data-ttu-id="39a72-204">如果 `member_access` 未分類為值，其 null 狀態會是「不是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-204">If a `member_access` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="39a72-205">否則，如果它是追蹤的運算式，其 null 狀態就是其在此來源位置的追蹤 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-205">Otherwise, if it is a tracked expression, its null state is its tracked null state at this source location.</span></span> <span data-ttu-id="39a72-206">否則，其 null 狀態為其類型的預設 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-206">Otherwise its null state is the default null state of its type.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="39a72-207">叫用運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-207">Invocation expressions</span></span>

<span data-ttu-id="39a72-208">如果 `invocation_expression` 叫用使用一或多個屬性所宣告的成員來進行特殊的 null 行為，則 null 狀態會由這些屬性決定。</span><span class="sxs-lookup"><span data-stu-id="39a72-208">If an `invocation_expression` invokes a member that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="39a72-209">否則，運算式的 null 狀態就是其類型的預設 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-209">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="element-access"></a><span data-ttu-id="39a72-210">元素存取</span><span class="sxs-lookup"><span data-stu-id="39a72-210">Element access</span></span>

<span data-ttu-id="39a72-211">如果 `element_access` 叫用使用一或多個屬性（attribute）宣告的索引子以進行特殊的 null 行為，則 null 狀態會由這些屬性所決定。</span><span class="sxs-lookup"><span data-stu-id="39a72-211">If an `element_access` invokes an indexer that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="39a72-212">否則，運算式的 null 狀態就是其類型的預設 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-212">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="base-access"></a><span data-ttu-id="39a72-213">基底存取</span><span class="sxs-lookup"><span data-stu-id="39a72-213">Base access</span></span>

<span data-ttu-id="39a72-214">如果 `B` 表示封入類型的基底類型，則 `base.I` 與 `((B)this).I` 具有相同的 null 狀態，而且 `base[E]` 與 `((B)this)[E]`具有相同的 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-214">If `B` denotes the base type of the enclosing type, `base.I` has the same null state as `((B)this).I` and `base[E]` has the same null state as `((B)this)[E]`.</span></span>

### <a name="default-expressions"></a><span data-ttu-id="39a72-215">預設運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-215">Default expressions</span></span>

<span data-ttu-id="39a72-216">如果已知 `T` 為不可為 null 數值型別，`default(T)` 具有 null 狀態「非 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-216">`default(T)` has the null state "non-null" if `T` is known to be a nonnullable value type.</span></span> <span data-ttu-id="39a72-217">否則，它會有 null 狀態「可能是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-217">Otherwise it has the null state "maybe null".</span></span>

### <a name="null-conditional-expressions"></a><span data-ttu-id="39a72-218">Null-條件運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-218">Null-conditional expressions</span></span>

<span data-ttu-id="39a72-219">`null_conditional_expression` 具有 null 狀態「可能是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-219">A `null_conditional_expression` has the null state "maybe null".</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="39a72-220">Cast 運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-220">Cast expressions</span></span>

<span data-ttu-id="39a72-221">如果 cast 運算式 `(T)E` 叫用使用者定義的轉換，則運算式的 null 狀態就是其類型的預設 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-221">If a cast expression `(T)E` invokes a user-defined conversion, then the null state of the expression is the default null state for its type.</span></span> <span data-ttu-id="39a72-222">否則，如果 `T` 是 null （*可為*null 或*未知*），則 null 狀態會是「可能是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-222">Otherwise, if `T` is null-yielding (*nullable* or *unknown*) then the null state is "maybe null".</span></span> <span data-ttu-id="39a72-223">否則，null 狀態會與 `E`的 null 狀態相同。</span><span class="sxs-lookup"><span data-stu-id="39a72-223">Otherwise the null state is the same as the null state of `E`.</span></span>

### <a name="await-expressions"></a><span data-ttu-id="39a72-224">Await 運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-224">Await expressions</span></span>

<span data-ttu-id="39a72-225">`await E` 的 null 狀態是其類型的預設 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-225">The null state of `await E` is the default null state of its type.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="39a72-226">`as` 運算子</span><span class="sxs-lookup"><span data-stu-id="39a72-226">The `as` operator</span></span>

<span data-ttu-id="39a72-227">`as` 運算式的狀態為「可能是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-227">An `as` expression has the null state "maybe null".</span></span>

### <a name="the-null-coalescing-operator"></a><span data-ttu-id="39a72-228">Null 聯合運算子</span><span class="sxs-lookup"><span data-stu-id="39a72-228">The null-coalescing operator</span></span>

<span data-ttu-id="39a72-229">`E1 ?? E2` 具有與 `E2` 相同的 null 狀態</span><span class="sxs-lookup"><span data-stu-id="39a72-229">`E1 ?? E2` has the same null state as `E2`</span></span>

### <a name="the-conditional-operator"></a><span data-ttu-id="39a72-230">條件運算子</span><span class="sxs-lookup"><span data-stu-id="39a72-230">The conditional operator</span></span>

<span data-ttu-id="39a72-231">如果 `E2` 和 `E3` 的 null 狀態為 "not null"，則 `E1 ? E2 : E3` 的 null 狀態為 "not null"。</span><span class="sxs-lookup"><span data-stu-id="39a72-231">The null state of `E1 ? E2 : E3` is "not null" if the null state of both `E2` and `E3` are "not null".</span></span> <span data-ttu-id="39a72-232">否則就是「可能是 null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-232">Otherwise it is "maybe null".</span></span>

### <a name="query-expressions"></a><span data-ttu-id="39a72-233">查詢運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-233">Query expressions</span></span>

<span data-ttu-id="39a72-234">查詢運算式的 null 狀態是其類型的預設 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-234">The null state of a query expression is the default null state of its type.</span></span>

### <a name="assignment-operators"></a><span data-ttu-id="39a72-235">指派運算子</span><span class="sxs-lookup"><span data-stu-id="39a72-235">Assignment operators</span></span>

<span data-ttu-id="39a72-236">套用任何隱含轉換之後，`E1 = E2` 和 `E1 op= E2` 與 `E2` 具有相同的 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-236">`E1 = E2` and `E1 op= E2` have the same null state as `E2` after any implicit conversions have been applied.</span></span>

### <a name="unary-and-binary-operators"></a><span data-ttu-id="39a72-237">一元和二元運算子</span><span class="sxs-lookup"><span data-stu-id="39a72-237">Unary and binary operators</span></span>

<span data-ttu-id="39a72-238">如果一元或二元運算子會叫用一個或多個屬性所宣告的使用者定義運算子，以進行特殊的 null 行為，則 null 狀態會由這些屬性決定。</span><span class="sxs-lookup"><span data-stu-id="39a72-238">If a unary or binary operator invokes an user-defined operator that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="39a72-239">否則，運算式的 null 狀態就是其類型的預設 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-239">Otherwise the null state of the expression is the default null state of its type.</span></span>

<span data-ttu-id="39a72-240">***在字串和委派上執行二進位 `+` 特別有用？***</span><span class="sxs-lookup"><span data-stu-id="39a72-240">***Something special to do for binary `+` over strings and delegates?***</span></span>

### <a name="expressions-that-propagate-null-state"></a><span data-ttu-id="39a72-241">傳播 null 狀態的運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-241">Expressions that propagate null state</span></span>

<span data-ttu-id="39a72-242">`(E)`、`checked(E)` 和 `unchecked(E)` 都具有與 `E`相同的 null 狀態。</span><span class="sxs-lookup"><span data-stu-id="39a72-242">`(E)`, `checked(E)` and `unchecked(E)` all have the same null state as `E`.</span></span>

### <a name="expressions-that-are-never-null"></a><span data-ttu-id="39a72-243">永不為 null 的運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-243">Expressions that are never null</span></span>

<span data-ttu-id="39a72-244">下列運算式形式的 null 狀態一律為 "not null"：</span><span class="sxs-lookup"><span data-stu-id="39a72-244">The null state of the following expression forms is always "not null":</span></span>

- <span data-ttu-id="39a72-245">`this` access</span><span class="sxs-lookup"><span data-stu-id="39a72-245">`this` access</span></span>
- <span data-ttu-id="39a72-246">字串插值</span><span class="sxs-lookup"><span data-stu-id="39a72-246">interpolated strings</span></span>
- <span data-ttu-id="39a72-247">`new` 運算式（物件、委派、匿名物件和陣列建立運算式）</span><span class="sxs-lookup"><span data-stu-id="39a72-247">`new` expressions (object, delegate, anonymous object and array creation expressions)</span></span>
- <span data-ttu-id="39a72-248">`typeof` 運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-248">`typeof` expressions</span></span>
- <span data-ttu-id="39a72-249">`nameof` 運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-249">`nameof` expressions</span></span>
- <span data-ttu-id="39a72-250">匿名函式（匿名方法和 lambda 運算式）</span><span class="sxs-lookup"><span data-stu-id="39a72-250">anonymous functions (anonymous methods and lambda expressions)</span></span>
- <span data-ttu-id="39a72-251">null-容許運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-251">null-forgiving expressions</span></span>
- <span data-ttu-id="39a72-252">`is` 運算式</span><span class="sxs-lookup"><span data-stu-id="39a72-252">`is` expressions</span></span>

## <a name="type-inference"></a><span data-ttu-id="39a72-253">型別推斷</span><span class="sxs-lookup"><span data-stu-id="39a72-253">Type inference</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="39a72-254">`var` 的型別推斷</span><span class="sxs-lookup"><span data-stu-id="39a72-254">Type inference for `var`</span></span>

<span data-ttu-id="39a72-255">針對以 `var` 宣告的區域變數所推斷的類型，會由初始化運算式的 null 狀態來通知。</span><span class="sxs-lookup"><span data-stu-id="39a72-255">The type inferred for local variables declared with `var` is informed by the null state of the initializing expression.</span></span>

```csharp
var x = E;
```

<span data-ttu-id="39a72-256">如果 `E` 的類型是可為 null 的參考型別 `C?` 而且 `E` 的 null 狀態為 "not null"，則會 `C`為 `x` 推斷的類型。</span><span class="sxs-lookup"><span data-stu-id="39a72-256">If the type of `E` is a nullable reference type `C?` and the null state of `E` is "not null" then the type inferred for `x` is `C`.</span></span> <span data-ttu-id="39a72-257">否則，推斷的類型會是 `E`的類型。</span><span class="sxs-lookup"><span data-stu-id="39a72-257">Otherwise, the inferred type is the type of `E`.</span></span>

<span data-ttu-id="39a72-258">針對 `x` 所推斷之類型的 null 屬性會根據 `var`的注釋內容來決定，就如同在該位置明確指定類型一樣。</span><span class="sxs-lookup"><span data-stu-id="39a72-258">The nullability of the type inferred for `x` is determined as described above, based on the annotation context of the `var`, just as if the type had been given explicitly in that position.</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="39a72-259">`var?` 的型別推斷</span><span class="sxs-lookup"><span data-stu-id="39a72-259">Type inference for `var?`</span></span>

<span data-ttu-id="39a72-260">針對以 `var?` 宣告的區域變數所推斷的類型，與初始化運算式的 null 狀態無關。</span><span class="sxs-lookup"><span data-stu-id="39a72-260">The type inferred for local variables declared with `var?` is independent of the null state of the initializing expression.</span></span>

```csharp
var? x = E;
```

<span data-ttu-id="39a72-261">如果 `E` 的型別 `T` 是可為 null 的實值型別或可為 null 的參考型別，則會 `T`為 `x` 推斷的型別。</span><span class="sxs-lookup"><span data-stu-id="39a72-261">If the type `T` of `E` is a nullable value type or a nullable reference type then the type inferred for `x` is `T`.</span></span> <span data-ttu-id="39a72-262">否則，如果 `T` 是不可為 null 實值型別，`S` 推斷的型別會 `S?`。</span><span class="sxs-lookup"><span data-stu-id="39a72-262">Otherwise, if `T` is a nonnullable value type `S` the type inferred is `S?`.</span></span> <span data-ttu-id="39a72-263">否則，如果 `T` 是不可為 null 參考型別，`C` 推斷的型別會 `C?`。</span><span class="sxs-lookup"><span data-stu-id="39a72-263">Otherwise, if `T` is a nonnullable reference type `C` the type inferred is `C?`.</span></span> <span data-ttu-id="39a72-264">否則，宣告是不合法的。</span><span class="sxs-lookup"><span data-stu-id="39a72-264">Otherwise, the declaration is illegal.</span></span>

<span data-ttu-id="39a72-265">針對 `x` 所推斷之型別的 null 屬性一律為*nullable*。</span><span class="sxs-lookup"><span data-stu-id="39a72-265">The nullability of the type inferred for `x` is always *nullable*.</span></span>

### <a name="generic-type-inference"></a><span data-ttu-id="39a72-266">泛型型別推斷</span><span class="sxs-lookup"><span data-stu-id="39a72-266">Generic type inference</span></span>

<span data-ttu-id="39a72-267">泛型型別推斷已增強，可協助決定推斷的參考型別是否應為可為 null。</span><span class="sxs-lookup"><span data-stu-id="39a72-267">Generic type inference is enhanced to help decide whether inferred reference types should be nullable or not.</span></span> <span data-ttu-id="39a72-268">這是最好的做法，而且本身不會產生警告，但當所選多載的推斷類型套用至引數時，可能會導致可為 null 的警告。</span><span class="sxs-lookup"><span data-stu-id="39a72-268">This is a best effort, and does not in and of itself yield warnings, but may lead to nullable warnings when the inferred types of the selected overload are applied to the arguments.</span></span>

<span data-ttu-id="39a72-269">型別推斷不依賴傳入類型的注釋內容。</span><span class="sxs-lookup"><span data-stu-id="39a72-269">The type inference does not rely on the annotation context of incoming types.</span></span> <span data-ttu-id="39a72-270">相反地，會推斷 `type`，它會從它的「原本就」（如果已明確表示），取得自己的注釋內容。</span><span class="sxs-lookup"><span data-stu-id="39a72-270">Instead a `type` is inferred which acquires its own annotation context from where it "would have been" if it had been expressed explicitly.</span></span> <span data-ttu-id="39a72-271">這會將型別推斷的角色底線，以方便您自行撰寫。</span><span class="sxs-lookup"><span data-stu-id="39a72-271">This underscores the role of type inference as a convenience for what you could have written yourself.</span></span>

<span data-ttu-id="39a72-272">更精確地說，推斷的型別引數的注釋內容是 token 的內容，此標記後面會接著 `<...>` 型別參數清單，還有一個。也就是所呼叫的泛型方法名稱。</span><span class="sxs-lookup"><span data-stu-id="39a72-272">More precisely, the annotation context for an inferred type argument is the context of the token that would have been followed by the `<...>` type parameter list, had there been one; i.e. the name of the generic method being called.</span></span> <span data-ttu-id="39a72-273">針對轉譯為這類呼叫的查詢運算式，會從產生呼叫之查詢子句的初始內容關鍵字取得內容。</span><span class="sxs-lookup"><span data-stu-id="39a72-273">For query expressions that translate to such calls, the context is taken from the initial contextual keyword of the query clause from which the call is generated.</span></span>

### <a name="the-first-phase"></a><span data-ttu-id="39a72-274">第一個階段</span><span class="sxs-lookup"><span data-stu-id="39a72-274">The first phase</span></span>

<span data-ttu-id="39a72-275">可為 null 的參考型別會流入初始運算式的界限，如下所述。</span><span class="sxs-lookup"><span data-stu-id="39a72-275">Nullable reference types flow into the bounds from the initial expressions, as described below.</span></span> <span data-ttu-id="39a72-276">此外，也會引進兩種新的界限，也就是 `null` 和 `default`。</span><span class="sxs-lookup"><span data-stu-id="39a72-276">In addition, two new kinds of bounds, namely `null` and `default` are introduced.</span></span> <span data-ttu-id="39a72-277">其目的是要執行輸入運算式中出現的 `null` 或 `default`，這可能會導致推斷的類型可為 null，即使不是也一樣。</span><span class="sxs-lookup"><span data-stu-id="39a72-277">Their purpose is to carry through occurrences of `null` or `default` in the input expressions, which may cause an inferred type to be nullable, even when it otherwise wouldn't.</span></span> <span data-ttu-id="39a72-278">這甚至適用于可為 null 的實*值*型別，其增強為在推斷程式中挑選「null」。</span><span class="sxs-lookup"><span data-stu-id="39a72-278">This works even for nullable *value* types, which are enhanced to pick up "nullness" in the inference process.</span></span>

<span data-ttu-id="39a72-279">決定要在第一個階段中新增的界限，其增強方式如下：</span><span class="sxs-lookup"><span data-stu-id="39a72-279">The determination of what bounds to add in the first phase are enhanced as follows:</span></span>

<span data-ttu-id="39a72-280">如果 `Ei` 的引數具有參考型別，則用於推斷的型別 `U` 會根據 `Ei` 的 null 狀態以及其宣告的型別而定：</span><span class="sxs-lookup"><span data-stu-id="39a72-280">If an argument `Ei` has a reference type, the type `U` used for inference depends on the null state of `Ei` as well as its declared type:</span></span>
- <span data-ttu-id="39a72-281">如果宣告的類型是不可為 null 參考型別 `U0` 或可為 null 的參考型別 `U0?` 則為</span><span class="sxs-lookup"><span data-stu-id="39a72-281">If the declared type is a nonnullable reference type `U0` or a nullable reference type `U0?` then</span></span>
    - <span data-ttu-id="39a72-282">如果 `Ei` 的 null 狀態為 "not null"，則 `U` 會 `U0`</span><span class="sxs-lookup"><span data-stu-id="39a72-282">if the null state of `Ei` is "not null" then `U` is `U0`</span></span>
    - <span data-ttu-id="39a72-283">如果 `Ei` 的 null 狀態是「可能是 null」，則 `U` 會 `U0?`</span><span class="sxs-lookup"><span data-stu-id="39a72-283">if the null state of `Ei` is "maybe null" then `U` is `U0?`</span></span>
- <span data-ttu-id="39a72-284">否則，如果 `Ei` 有已宣告的型別，`U` 就是該型別</span><span class="sxs-lookup"><span data-stu-id="39a72-284">Otherwise if `Ei` has a declared type, `U` is that type</span></span>
- <span data-ttu-id="39a72-285">否則，如果 `Ei` `null`，則 `U` 是特殊界限 `null`</span><span class="sxs-lookup"><span data-stu-id="39a72-285">Otherwise if `Ei` is `null` then `U` is the special bound `null`</span></span>
- <span data-ttu-id="39a72-286">否則，如果 `Ei` `default`，則 `U` 是特殊界限 `default`</span><span class="sxs-lookup"><span data-stu-id="39a72-286">Otherwise if `Ei` is `default` then `U` is the special bound `default`</span></span>
- <span data-ttu-id="39a72-287">否則不會進行推斷。</span><span class="sxs-lookup"><span data-stu-id="39a72-287">Otherwise no inference is made.</span></span>

### <a name="exact-upper-bound-and-lower-bound-inferences"></a><span data-ttu-id="39a72-288">精確、上限和下限推斷</span><span class="sxs-lookup"><span data-stu-id="39a72-288">Exact, upper-bound and lower-bound inferences</span></span>

<span data-ttu-id="39a72-289">*從*類型 `U` 推斷*到*類型 `V`，如果 `V` 是可為 null 的參考型別 `V0?`，則會使用 `V0`，而不是在下列子句中 `V`。</span><span class="sxs-lookup"><span data-stu-id="39a72-289">In inferences *from* the type `U` *to* the type `V`, if `V` is a nullable reference type `V0?`, then `V0` is used instead of `V` in the following clauses.</span></span>
- <span data-ttu-id="39a72-290">如果 `V` 是其中一個未固定的類型變數，`U` 會加入為精確、上限或下限，如同之前一樣</span><span class="sxs-lookup"><span data-stu-id="39a72-290">If `V` is one of the unfixed type variables, `U` is added as an exact, upper or lower bound as before</span></span>
- <span data-ttu-id="39a72-291">否則，如果 `U` `null` 或 `default`，則不會進行任何推斷</span><span class="sxs-lookup"><span data-stu-id="39a72-291">Otherwise, if `U` is `null` or `default`, no inference is made</span></span>
- <span data-ttu-id="39a72-292">否則，如果 `U` 是可為 null 的參考型別 `U0?`，則會使用 `U0`，而不是後續子句中的 `U`。</span><span class="sxs-lookup"><span data-stu-id="39a72-292">Otherwise, if `U` is a nullable reference type `U0?`, then `U0` is used instead of `U` in the subsequent clauses.</span></span>

<span data-ttu-id="39a72-293">基本上，其本質上是直接與其中一個非固定類型變數相關的 null 屬性會保留在其界限內。</span><span class="sxs-lookup"><span data-stu-id="39a72-293">The essence is that nullability that pertains directly to one of the unfixed type variables is preserved into its bounds.</span></span> <span data-ttu-id="39a72-294">另一方面，若要推斷會進一步遞迴進入來源和目標型別，則會忽略 null 屬性。</span><span class="sxs-lookup"><span data-stu-id="39a72-294">For the inferences that recurse further into the source and target types, on the other hand, nullability is ignored.</span></span> <span data-ttu-id="39a72-295">它不一定會相符，但如果沒有，則會在稍後選擇並套用多載時發出警告。</span><span class="sxs-lookup"><span data-stu-id="39a72-295">It may or may not match, but if it doesn't, a warning will be issued later if the overload is chosen and applied.</span></span>

### <a name="fixing"></a><span data-ttu-id="39a72-296">修正</span><span class="sxs-lookup"><span data-stu-id="39a72-296">Fixing</span></span>

<span data-ttu-id="39a72-297">此規格目前並不會說明當多個界限互相轉換，但不同時，會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="39a72-297">The spec currently does not do a good job of describing what happens when multiple bounds are identity convertible to each other, but are different.</span></span> <span data-ttu-id="39a72-298">這可能會發生在 `object` 和 `dynamic`之間，而不同于元素名稱的元組類型之間，兩者之間的類型之間，而且現在也會在參考型別的 `C` 和 `C?` 之間進行。</span><span class="sxs-lookup"><span data-stu-id="39a72-298">This may happen between `object` and `dynamic`, between tuple types that differ only in element names, between types constructed thereof and now also between `C` and `C?` for reference types.</span></span>

<span data-ttu-id="39a72-299">此外，我們還需要將 "null" 從輸入運算式傳播到結果型別。</span><span class="sxs-lookup"><span data-stu-id="39a72-299">In addition we need to propagate "nullness" from the input expressions to the result type.</span></span> 

<span data-ttu-id="39a72-300">為了處理這些情況，我們新增了更多階段來修正，這現在是：</span><span class="sxs-lookup"><span data-stu-id="39a72-300">To handle these we add more phases to fixing, which is now:</span></span>

1. <span data-ttu-id="39a72-301">將所有界限中的所有類型收集為候選項目，從所有可為 null 的參考型別移除 `?`</span><span class="sxs-lookup"><span data-stu-id="39a72-301">Gather all the types in all the bounds as candidates, removing `?` from all that are nullable reference types</span></span>
2. <span data-ttu-id="39a72-302">根據精確、較低和上限的需求來排除候選人（保留 `null` 和 `default` 界限）</span><span class="sxs-lookup"><span data-stu-id="39a72-302">Eliminate candidates based on requirements of exact, lower and upper bounds (keeping `null` and `default` bounds)</span></span>
3. <span data-ttu-id="39a72-303">排除沒有隱含轉換至所有其他候選項目的候選項目</span><span class="sxs-lookup"><span data-stu-id="39a72-303">Eliminate candidates that do not have an implicit conversion to all the other candidates</span></span>
4. <span data-ttu-id="39a72-304">如果其餘的候選項目都不會彼此進行身分識別轉換，則型別推斷會失敗</span><span class="sxs-lookup"><span data-stu-id="39a72-304">If the remaining candidates do not all have identity conversions to one another, then type inference fails</span></span>
5. <span data-ttu-id="39a72-305">*合併*剩餘的候選項目，如下所述</span><span class="sxs-lookup"><span data-stu-id="39a72-305">*Merge* the remaining candidates as described below</span></span>
6. <span data-ttu-id="39a72-306">如果產生的候選項是參考型別或不可為 null 值型別，而且*所有*的確切界限或*任何*下限都是可為 null 的實數值型別、可為 null 的參考型別、`null` 或 `default`，則 `?` 會加入至產生的候選項，使其成為可為 null 的實數值型別或參考型別。</span><span class="sxs-lookup"><span data-stu-id="39a72-306">If the resulting candidate is a reference type or a nonnullable value type and *all* of the exact bounds or *any* of the lower bounds are nullable value types, nullable reference types, `null` or `default`, then `?` is added to the resulting candidate, making it a nullable value type or reference type.</span></span>

<span data-ttu-id="39a72-307">兩個候選類型之間會描述*合併*。</span><span class="sxs-lookup"><span data-stu-id="39a72-307">*Merging* is described between two candidate types.</span></span> <span data-ttu-id="39a72-308">它是可轉移和交換的，因此可以使用相同的最終結果，以任何順序合併候選項目。</span><span class="sxs-lookup"><span data-stu-id="39a72-308">It is transitive and commutative, so the candidates can be merged in any order with the same ultimate result.</span></span> <span data-ttu-id="39a72-309">如果兩個候選型別無法彼此轉換，則未定義。</span><span class="sxs-lookup"><span data-stu-id="39a72-309">It is undefined if the two candidate types are not identity convertible to each other.</span></span>

<span data-ttu-id="39a72-310">*Merge*函數接受兩個候選類型和一個方向（ *+* 或 *-* ）：</span><span class="sxs-lookup"><span data-stu-id="39a72-310">The *Merge* function takes two candidate types and a direction (*+* or *-*):</span></span>

- <span data-ttu-id="39a72-311">*Merge*（`T`、`T`、 *d*） = t</span><span class="sxs-lookup"><span data-stu-id="39a72-311">*Merge*(`T`, `T`, *d*) = T</span></span>
- <span data-ttu-id="39a72-312">*Merge*（`S`、`T?`、 *+* ） = *merge*（`S?`、`T`、 *+* ） = *merge*（`S`，`T`， *+* ）`?`</span><span class="sxs-lookup"><span data-stu-id="39a72-312">*Merge*(`S`, `T?`, *+*) = *Merge*(`S?`, `T`, *+*) = *Merge*(`S`, `T`, *+*)`?`</span></span>
- <span data-ttu-id="39a72-313">*Merge*（`S`，`T?`， *-* ） = *merge*（`S?`，`T`， *-* ） = *merge*（`S`，`T`， *-* ）</span><span class="sxs-lookup"><span data-stu-id="39a72-313">*Merge*(`S`, `T?`, *-*) = *Merge*(`S?`, `T`, *-*) = *Merge*(`S`, `T`, *-*)</span></span>
- <span data-ttu-id="39a72-314">*Merge*（`C<S1,...,Sn>`，`C<T1,...,Tn>`， *+* ） = `C<`*merge*（`S1`，`T1`， *d1*）`,...,`*merge*（`Sn`，`Tn`， *dn*）`>`，*其中*</span><span class="sxs-lookup"><span data-stu-id="39a72-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="39a72-315">如果 `C<...>` 的 `i`' 第一個類型參數為協變數，則 `di` =  *+*</span><span class="sxs-lookup"><span data-stu-id="39a72-315">`di` = *+* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="39a72-316">如果 `C<...>` 的 `i`' 第一個類型參數為非變異或不變，則 `di` =  *-*</span><span class="sxs-lookup"><span data-stu-id="39a72-316">`di` = *-* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="39a72-317">*Merge*（`C<S1,...,Sn>`，`C<T1,...,Tn>`， *-* ） = `C<`*merge*（`S1`，`T1`， *d1*）`,...,`*merge*（`Sn`，`Tn`， *dn*）`>`，*其中*</span><span class="sxs-lookup"><span data-stu-id="39a72-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="39a72-318">如果 `C<...>` 的 `i`' 第一個類型參數為協變數，則 `di` =  *-*</span><span class="sxs-lookup"><span data-stu-id="39a72-318">`di` = *-* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="39a72-319">如果 `C<...>` 的 `i`' 第一個類型參數為非變異或不變，則 `di` =  *+*</span><span class="sxs-lookup"><span data-stu-id="39a72-319">`di` = *+* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="39a72-320">*Merge*（`(S1 s1,..., Sn sn)`、`(T1 t1,..., Tn tn)`、 *d*） = `(`*merge*（`S1`、`T1`、 *d*）`n1,...,`*merge*（`Sn`，`Tn`， *d*） `nn)`，*其中*</span><span class="sxs-lookup"><span data-stu-id="39a72-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*Merge*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *where*</span></span>
    - <span data-ttu-id="39a72-321">如果 `si` 和 `ti` 不同，或兩者都不存在，則 `ni` 不存在</span><span class="sxs-lookup"><span data-stu-id="39a72-321">`ni` is absent if `si` and `ti` differ, or if both are absent</span></span>
    - <span data-ttu-id="39a72-322">如果 `si` 和 `ti` 相同，則會 `si` `ni`</span><span class="sxs-lookup"><span data-stu-id="39a72-322">`ni` is `si` if `si` and `ti` are the same</span></span>
- <span data-ttu-id="39a72-323">*Merge*（`object`，`dynamic`） = *merge*（`dynamic`，`object`） = `dynamic`</span><span class="sxs-lookup"><span data-stu-id="39a72-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span></span>

## <a name="warnings"></a><span data-ttu-id="39a72-324">警告</span><span class="sxs-lookup"><span data-stu-id="39a72-324">Warnings</span></span>

### <a name="potential-null-assignment"></a><span data-ttu-id="39a72-325">可能的 null 指派</span><span class="sxs-lookup"><span data-stu-id="39a72-325">Potential null assignment</span></span>

### <a name="potential-null-dereference"></a><span data-ttu-id="39a72-326">可能的 null 取值</span><span class="sxs-lookup"><span data-stu-id="39a72-326">Potential null dereference</span></span>

### <a name="constraint-nullability-mismatch"></a><span data-ttu-id="39a72-327">條件約束 null 屬性不相符</span><span class="sxs-lookup"><span data-stu-id="39a72-327">Constraint nullability mismatch</span></span>

### <a name="nullable-types-in-disabled-annotation-context"></a><span data-ttu-id="39a72-328">已停用注釋內容中可為 null 的類型</span><span class="sxs-lookup"><span data-stu-id="39a72-328">Nullable types in disabled annotation context</span></span>

## <a name="attributes-for-special-null-behavior"></a><span data-ttu-id="39a72-329">特殊 null 行為的屬性</span><span class="sxs-lookup"><span data-stu-id="39a72-329">Attributes for special null behavior</span></span>


