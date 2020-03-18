---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2019
ms.locfileid: "79485123"
---

# <a name="discriminated-unions--enum-class"></a><span data-ttu-id="72d08-101">區分聯集/`enum class`</span><span class="sxs-lookup"><span data-stu-id="72d08-101">Discriminated unions / `enum class`</span></span>

<span data-ttu-id="72d08-102">`enum class`es 是一種新類型的宣告，有時稱為「區分等位」，其中每個可能的實例都會列出該類型，而且每個實例都是非重迭。</span><span class="sxs-lookup"><span data-stu-id="72d08-102">`enum class`es are a new kind of type declaration, sometimes referred to as discriminated unions, where each every possible instance the type is listed, and each instance is non-overlapping.</span></span>

<span data-ttu-id="72d08-103">`enum class` 是使用下列語法定義的：</span><span class="sxs-lookup"><span data-stu-id="72d08-103">An `enum class` is defined using the following syntax:</span></span>

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

<span data-ttu-id="72d08-104">範例語法：</span><span class="sxs-lookup"><span data-stu-id="72d08-104">Sample syntax:</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a><span data-ttu-id="72d08-105">語意</span><span class="sxs-lookup"><span data-stu-id="72d08-105">Semantics</span></span>

<span data-ttu-id="72d08-106">`enum class` 定義會定義根類型，這是與 `enum class` 宣告同名的抽象類別，以及一組成員，其中每一個都有類型，也就是根類型的子類型。</span><span class="sxs-lookup"><span data-stu-id="72d08-106">An `enum class` definition defines a root type, which is an abstract class of the same name as the `enum class` declaration, and a set of members, each of which has a type which is a subtype of the root type.</span></span> <span data-ttu-id="72d08-107">如果有多個 `partial enum class` 定義，則所有成員都會被視為列舉類別定義的成員。</span><span class="sxs-lookup"><span data-stu-id="72d08-107">If there are multiple `partial enum class` definitions, all members will be considered members of the enum class definition.</span></span> <span data-ttu-id="72d08-108">不同于使用者定義的抽象類別定義，`enum class` 根類型預設為部分，並定義為具有預設的*私*用無參數的函式。</span><span class="sxs-lookup"><span data-stu-id="72d08-108">Unlike a user-defined abstract class definition, the `enum class` root type is partial by default and defined to have a default *private* parameter-less constructor.</span></span>

<span data-ttu-id="72d08-109">請注意，由於根類型已定義為部分抽象類別，因此也可以加入*根類型*的部分定義，其中會允許類別主體的標準語法形式。</span><span class="sxs-lookup"><span data-stu-id="72d08-109">Note that, since the root type is defined to be a partial abstract class, partial definitions of the *root type* may also be added, where standard syntax forms for a class body are allowed.</span></span>
<span data-ttu-id="72d08-110">不過，除了指定為 `enum class` 成員的任何宣告之外，沒有任何類型可以直接繼承自根類型。</span><span class="sxs-lookup"><span data-stu-id="72d08-110">However, no types may directly inherit from the root type in any declaration, aside from those specified as `enum class` members.</span></span> <span data-ttu-id="72d08-111">此外，根類型也不允許使用者定義的函數。</span><span class="sxs-lookup"><span data-stu-id="72d08-111">In addition, no user-defined constructors are permitted for the root type.</span></span>

<span data-ttu-id="72d08-112">有四種類型的 `enum class` 成員宣告：</span><span class="sxs-lookup"><span data-stu-id="72d08-112">There are four kinds of `enum class` member declarations:</span></span>

* <span data-ttu-id="72d08-113">簡單類別成員</span><span class="sxs-lookup"><span data-stu-id="72d08-113">simple class members</span></span>

* <span data-ttu-id="72d08-114">複雜類別成員</span><span class="sxs-lookup"><span data-stu-id="72d08-114">complex class members</span></span>

* <span data-ttu-id="72d08-115">`enum class` 成員</span><span class="sxs-lookup"><span data-stu-id="72d08-115">`enum class` members</span></span>

* <span data-ttu-id="72d08-116">值成員。</span><span class="sxs-lookup"><span data-stu-id="72d08-116">value members.</span></span>

### <a name="simple-class-members"></a><span data-ttu-id="72d08-117">簡單類別成員</span><span class="sxs-lookup"><span data-stu-id="72d08-117">Simple class members</span></span>

<span data-ttu-id="72d08-118">簡單的類別成員宣告會定義具有相同名稱的新的嵌套「記錄」類別（在本檔中刻意保持未定義狀態）。</span><span class="sxs-lookup"><span data-stu-id="72d08-118">A simple class member declaration defines a new nested "record" class (intentionally left undefined in this document) with the same name.</span></span> <span data-ttu-id="72d08-119">Nested 類別繼承自根型別。</span><span class="sxs-lookup"><span data-stu-id="72d08-119">The nested class inherits from the root type.</span></span>

<span data-ttu-id="72d08-120">假設上述範例程式碼，</span><span class="sxs-lookup"><span data-stu-id="72d08-120">Given the sample code above,</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

<span data-ttu-id="72d08-121">`enum class` 宣告的語義等同于下列宣告</span><span class="sxs-lookup"><span data-stu-id="72d08-121">the `enum class` declaration has semantics equivalent to the following declaration</span></span>

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a><span data-ttu-id="72d08-122">複雜類別成員</span><span class="sxs-lookup"><span data-stu-id="72d08-122">Complex class members</span></span>

<span data-ttu-id="72d08-123">您也可以在 `enum class` 宣告底下，嵌套整個類別宣告。</span><span class="sxs-lookup"><span data-stu-id="72d08-123">You can also nest an entire class declaration below an `enum class` declaration.</span></span> <span data-ttu-id="72d08-124">它會被視為根類型的嵌套類別。</span><span class="sxs-lookup"><span data-stu-id="72d08-124">It will be treated as a nested class of the root type.</span></span> <span data-ttu-id="72d08-125">語法允許任何類別宣告，但複雜類別成員必須從直接包含 `enum class` 宣告繼承。</span><span class="sxs-lookup"><span data-stu-id="72d08-125">The syntax allows any class declaration, but it is required for the complex class member to inherit from the direct containing `enum class` declaration.</span></span> 

### <a name="enum-class-members"></a><span data-ttu-id="72d08-126">`enum class` 成員</span><span class="sxs-lookup"><span data-stu-id="72d08-126">`enum class` members</span></span>

<span data-ttu-id="72d08-127">`enum classes` 可以彼此嵌套，例如</span><span class="sxs-lookup"><span data-stu-id="72d08-127">`enum classes` can be nested under each other, e.g.</span></span>

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

<span data-ttu-id="72d08-128">這幾乎與最上層 `enum class`的語義相同，不同之處在于嵌套的 enum 類別會定義嵌套根類型，而嵌套的 enum 類別底下的所有專案，都是嵌套根類型的子類型，而不是最上層的根類型。</span><span class="sxs-lookup"><span data-stu-id="72d08-128">This is almost identical to the semantics of a top-level `enum class`, except that the nested enum class defines a nested root type, and everything below the nested enum class is a subtype of the nested root type, instead of the top-level root type.</span></span>

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a><span data-ttu-id="72d08-129">值成員</span><span class="sxs-lookup"><span data-stu-id="72d08-129">Value members</span></span>

<span data-ttu-id="72d08-130">`enum classes` 也可以包含值成員。</span><span class="sxs-lookup"><span data-stu-id="72d08-130">`enum classes` can also contain value members.</span></span> <span data-ttu-id="72d08-131">值成員會在根型別上定義公用的僅取得靜態屬性，同時也會傳回根型別，例如</span><span class="sxs-lookup"><span data-stu-id="72d08-131">Value members define public get-only static properties on the root type that also return the root type, e.g.</span></span>

```C#
enum class Color
{
    Red,
    Green
}
```

<span data-ttu-id="72d08-132">的屬性相當於</span><span class="sxs-lookup"><span data-stu-id="72d08-132">has properties equivalent to</span></span>

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

<span data-ttu-id="72d08-133">完整的語義會被視為一個執行詳細資料，但保證每個屬性都會傳回一個唯一的實例，而且重複叫用時也會傳回相同的實例。</span><span class="sxs-lookup"><span data-stu-id="72d08-133">The complete semantics are considered an implementation detail, but it is guaranteed that one unique instance will be returned for each property, and the same instance will be returned on repeated invocations.</span></span>


### <a name="switch-expression-and-patterns"></a><span data-ttu-id="72d08-134">Switch 運算式和模式</span><span class="sxs-lookup"><span data-stu-id="72d08-134">Switch expression and patterns</span></span>

<span data-ttu-id="72d08-135">有一些建議的模式比對和切換運算式的調整，以處理 `enum classes`。</span><span class="sxs-lookup"><span data-stu-id="72d08-135">There are some proposed adjustments to pattern matching and the switch expression to handle `enum classes`.</span></span> <span data-ttu-id="72d08-136">Switch 運算式已經透過變數模式來比對型別，但目前針對引用型別，switch 運算式中沒有任何一組切換臂被視為完整，除了比對引數的靜態型別或子型別。</span><span class="sxs-lookup"><span data-stu-id="72d08-136">Switch expressions can already match types through the variable pattern, but for currently for reference types, no set of switch arms in the switch expression are considered complete, except for matching against the static type of the argument, or a subtype.</span></span>

<span data-ttu-id="72d08-137">Switch 運算式會進行變更，如此一來，如果 `enum class` 的根類型是 switch 運算式之引數的靜態類型，而且有一組模式符合列舉的所有成員，則參數會被視為完整的。</span><span class="sxs-lookup"><span data-stu-id="72d08-137">Switch expressions would be changed such that, if the root type of an `enum class` is the static type of the argument to the switch expression, and there is a set of patterns matching all members of the enum, then the switch will be considered exhaustive.</span></span>

<span data-ttu-id="72d08-138">由於值成員不是常數，而且不會定義新的靜態類型，因此目前無法依模式來比對。</span><span class="sxs-lookup"><span data-stu-id="72d08-138">Since value members are not constants and do not define new static types, they currently cannot be matched by pattern.</span></span> <span data-ttu-id="72d08-139">若要這麼做，將會加入使用常數模式語法的新模式，以允許與 `enum class` 值成員進行比對。</span><span class="sxs-lookup"><span data-stu-id="72d08-139">To make this possible, a new pattern using the constant pattern syntax will be added to allow match against `enum class` value members.</span></span> <span data-ttu-id="72d08-140">只有當模式比對的引數與 `enum class` 值成員所傳回的值相等時，才會將比對定義為成功，雖然不需要執行這項檢查。</span><span class="sxs-lookup"><span data-stu-id="72d08-140">The match is defined to succeed if and only if the argument to the pattern match and the value returned by the `enum class` value member would be reference equal, although the implementation is not required to perform this check.</span></span>


## <a name="open-questions"></a><span data-ttu-id="72d08-141">開啟問題</span><span class="sxs-lookup"><span data-stu-id="72d08-141">Open questions</span></span>

- <span data-ttu-id="72d08-142">[] 一般類型演算法對 `enum class` 成員的看法為何？</span><span class="sxs-lookup"><span data-stu-id="72d08-142">[ ] What does the common type algorithm say about `enum class` members?</span></span> <span data-ttu-id="72d08-143">這是有效的程式碼嗎？</span><span class="sxs-lookup"><span data-stu-id="72d08-143">Is this valid code?</span></span>
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- <span data-ttu-id="72d08-144">[] 只為值成員加入新的模式似乎很繁重。</span><span class="sxs-lookup"><span data-stu-id="72d08-144">[ ] Adding a new pattern just for value members seems heavy handed.</span></span> <span data-ttu-id="72d08-145">是否有更通用的版本結構，這有意義？</span><span class="sxs-lookup"><span data-stu-id="72d08-145">Is there a more general version construction that makes sense?</span></span>
    - <span data-ttu-id="72d08-146">[] 值成員也無法正確對應至平行的嵌套類別結構，因為</span><span class="sxs-lookup"><span data-stu-id="72d08-146">[ ] Value members also do not map well to a parallel nested class construction because of this</span></span>

- <span data-ttu-id="72d08-147">[] 正在針對具有 `enum class` 靜態類型的引數進行切換，保證是固定時間？</span><span class="sxs-lookup"><span data-stu-id="72d08-147">[ ] Is switching against an argument with an `enum class` static type guaranteed to be constant-time?</span></span>

- <span data-ttu-id="72d08-148">[] 是否有方法可讓 `enum class`在 switch 運算式中不被視為已完成？</span><span class="sxs-lookup"><span data-stu-id="72d08-148">[ ] Should there be a way to make `enum class`es not be considered complete in the switch expression?</span></span> <span data-ttu-id="72d08-149">有 `virtual`的前置詞嗎？</span><span class="sxs-lookup"><span data-stu-id="72d08-149">Prefix with `virtual`?</span></span>

- <span data-ttu-id="72d08-150">[] `enum class`上應允許哪些修飾詞？</span><span class="sxs-lookup"><span data-stu-id="72d08-150">[ ] What modifiers should be permitted on `enum class`?</span></span>