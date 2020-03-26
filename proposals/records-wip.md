---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281953"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="1b113-101">記錄進行中的工作</span><span class="sxs-lookup"><span data-stu-id="1b113-101">Records Work-in-Progress</span></span>

<span data-ttu-id="1b113-102">不同于其他記錄提案，這不是本身的提案，而是針對記錄功能記錄共識設計決策的工作進行中。</span><span class="sxs-lookup"><span data-stu-id="1b113-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="1b113-103">系統會視需要新增規格詳細資料以解決問題。</span><span class="sxs-lookup"><span data-stu-id="1b113-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="1b113-104">建議新增記錄的語法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1b113-104">The syntax for a record is proposed to be added as follows:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

<span data-ttu-id="1b113-105">`attributes` 的非終端機也會允許新的內容屬性，`data`。</span><span class="sxs-lookup"><span data-stu-id="1b113-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="1b113-106">使用參數清單或 `data` 修飾詞宣告的類別（結構）稱為記錄類別（記錄結構），其中之一是記錄類型。</span><span class="sxs-lookup"><span data-stu-id="1b113-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="1b113-107">宣告不含參數清單和 `data` 修飾詞的記錄類型是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="1b113-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="1b113-108">記錄類型的成員</span><span class="sxs-lookup"><span data-stu-id="1b113-108">Members of a record type</span></span>

<span data-ttu-id="1b113-109">除了在類別主體中宣告的成員之外，記錄類型還有下列其他成員：</span><span class="sxs-lookup"><span data-stu-id="1b113-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="1b113-110">主要的構造函式</span><span class="sxs-lookup"><span data-stu-id="1b113-110">Primary Constructor</span></span>

<span data-ttu-id="1b113-111">記錄類型具有公用的函式，其簽章對應于類型宣告的值參數。</span><span class="sxs-lookup"><span data-stu-id="1b113-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="1b113-112">這稱為類型的主要「函式」，並會隱藏隱含宣告的預設函數。</span><span class="sxs-lookup"><span data-stu-id="1b113-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="1b113-113">具有主要的函式，以及具有相同簽章的相同簽章已存在於類別中，是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="1b113-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="1b113-114">在執行時間，主要的函式</span><span class="sxs-lookup"><span data-stu-id="1b113-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="1b113-115">執行出現在類別主體中的實例欄位初始化運算式;然後叫用沒有引數的基類處理函式。</span><span class="sxs-lookup"><span data-stu-id="1b113-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="1b113-116">針對對應至值參數的屬性，初始化編譯器產生的支援欄位（如果這些屬性是由編譯器提供，請參閱[合成的屬性](#Synthesized Properties)）</span><span class="sxs-lookup"><span data-stu-id="1b113-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="1b113-117">[] TODO：新增基底呼叫語法和規格，以透過多載解析選擇基底函數</span><span class="sxs-lookup"><span data-stu-id="1b113-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="1b113-118">屬性</span><span class="sxs-lookup"><span data-stu-id="1b113-118">Properties</span></span>

<span data-ttu-id="1b113-119">對於記錄類型宣告的每個記錄參數，有一個對應的公用屬性成員，其名稱和類型取自值參數宣告。</span><span class="sxs-lookup"><span data-stu-id="1b113-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="1b113-120">如果沒有具有 get 存取子的具象（非抽象）屬性，而且已明確宣告或繼承此名稱和類型，則編譯器會產生，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1b113-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="1b113-121">若為記錄結構或記錄類別：</span><span class="sxs-lookup"><span data-stu-id="1b113-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="1b113-122">建立公用的僅限自動屬性。</span><span class="sxs-lookup"><span data-stu-id="1b113-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="1b113-123">它的值會在結構化期間使用對應的主要函式參數值進行初始化。</span><span class="sxs-lookup"><span data-stu-id="1b113-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="1b113-124">會覆寫每個「相符」的繼承抽象屬性 get 存取子。</span><span class="sxs-lookup"><span data-stu-id="1b113-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="1b113-125">等號比較成員</span><span class="sxs-lookup"><span data-stu-id="1b113-125">Equality members</span></span>

<span data-ttu-id="1b113-126">記錄類型會產生下列方法的合成實作為：</span><span class="sxs-lookup"><span data-stu-id="1b113-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="1b113-127">`object.GetHashCode()` 覆寫，除非已密封或使用者提供</span><span class="sxs-lookup"><span data-stu-id="1b113-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="1b113-128">`object.Equals(object)` 覆寫，除非已密封或使用者提供</span><span class="sxs-lookup"><span data-stu-id="1b113-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="1b113-129">`T Equals(T)` 方法，其中 `T` 為目前的類型</span><span class="sxs-lookup"><span data-stu-id="1b113-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="1b113-130">指定 `T Equals(T)` 以執行實值相等性，並比較每個主要的函式參數與另一個類型之對應屬性的名稱相同的屬性。</span><span class="sxs-lookup"><span data-stu-id="1b113-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="1b113-131">`object.Equals` 執行的對等</span><span class="sxs-lookup"><span data-stu-id="1b113-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="1b113-132">`with` 運算式</span><span class="sxs-lookup"><span data-stu-id="1b113-132">`with` expression</span></span>

<span data-ttu-id="1b113-133">`with` 運算式是使用下列語法的新運算式。</span><span class="sxs-lookup"><span data-stu-id="1b113-133">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="1b113-134">`with` 運算式允許「非破壞性變化」，其設計目的是要產生接收者運算式的複本，並修改 `anonymous_object_initializer`中列出的屬性。</span><span class="sxs-lookup"><span data-stu-id="1b113-134">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="1b113-135">有效的 `with` 運算式具有非 void 類型的接收者。</span><span class="sxs-lookup"><span data-stu-id="1b113-135">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="1b113-136">接收器類型必須包含一個可存取的實例方法，稱為 `With` 並具有適當的參數和傳回型別。</span><span class="sxs-lookup"><span data-stu-id="1b113-136">The receiver type must contain an accessible instance method called `With` with the appropriate parameters and return type.</span></span> <span data-ttu-id="1b113-137">如果有多個非覆寫 `With` 方法，這就是錯誤。</span><span class="sxs-lookup"><span data-stu-id="1b113-137">It is an error if there are multiple non-override `With` methods.</span></span> <span data-ttu-id="1b113-138">如果有多個 `With` 覆寫，就必須有非覆寫 `With` 方法，這是目標方法。</span><span class="sxs-lookup"><span data-stu-id="1b113-138">If there are multiple `With` overrides, there must be a non-override `With` method, which is the target method.</span></span> <span data-ttu-id="1b113-139">否則，必須只有一個 `With` 方法。</span><span class="sxs-lookup"><span data-stu-id="1b113-139">Otherwise, there must be exactly one `With` method.</span></span>

<span data-ttu-id="1b113-140">在 `with` 運算式右側是一個 `anonymous_object_initializer`，其中具有一系列具有指派之欄位或屬性的指派，而右邊的任意運算式則可以隱含地轉換成左邊的型別，這是一個具有序列的指定。</span><span class="sxs-lookup"><span data-stu-id="1b113-140">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments with a field or property of the receiver on the left-hand side of the assignment, and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the left-hand side.</span></span>

<span data-ttu-id="1b113-141">假設目標 `With` 方法，則傳回類型必須是接收者運算式類型的類型，或其基底類型。</span><span class="sxs-lookup"><span data-stu-id="1b113-141">Given a target `With` method, the return type must be the type of the receiver expression type, or a base type thereof.</span></span> <span data-ttu-id="1b113-142">針對 `With` 方法的每個參數，在具有相同名稱和相同類型的接收者類型上，必須有可存取的對應實例欄位或可讀取的屬性。</span><span class="sxs-lookup"><span data-stu-id="1b113-142">For each parameter of the `With` method, there must be an accessible corresponding instance field or readable property on the receiver type with the same name and the same type.</span></span> <span data-ttu-id="1b113-143">With 運算式右側的每個屬性或欄位也必須對應至 `With` 方法中相同名稱的參數。</span><span class="sxs-lookup"><span data-stu-id="1b113-143">Each property or field in the right-hand side of the With expression must also correspond to a parameter of the same name in the `With` method.</span></span>

<span data-ttu-id="1b113-144">假設有有效的 `With` 方法，`with` 運算式的評估就相當於使用 `anonymous_object_initializer` 中的運算式來呼叫 `With` 方法，以取代與左側屬性相同名稱的參數。</span><span class="sxs-lookup"><span data-stu-id="1b113-144">Given a valid `With` method, the evaluation of a `with` expression is equivalent to calling the `With` method with the expressions in the `anonymous_object_initializer` substituted for the parameter of the same name as the property on the left hand side.</span></span> <span data-ttu-id="1b113-145">如果 `anonymous_object_initializer`中的指定參數沒有相符的屬性，則引數會評估接收者上相同名稱的欄位或屬性。</span><span class="sxs-lookup"><span data-stu-id="1b113-145">If there is no matching property for a given parameter in the `anonymous_object_initializer`, the argument is the evaluation of the field or property of the same name on the receiver.</span></span>

<span data-ttu-id="1b113-146">副作用的評估順序如下所示，每個運算式只會評估一次：</span><span class="sxs-lookup"><span data-stu-id="1b113-146">The order of evaluation of side effects is as follows, with each expression evaluated exactly once:</span></span>

1. <span data-ttu-id="1b113-147">接收者運算式</span><span class="sxs-lookup"><span data-stu-id="1b113-147">Receiver expression</span></span>

2. <span data-ttu-id="1b113-148">`anonymous_object_initializer`中的運算式（依詞法順序）</span><span class="sxs-lookup"><span data-stu-id="1b113-148">Expressions in the `anonymous_object_initializer`, in lexical order</span></span>

3. <span data-ttu-id="1b113-149">以 `With` 方法參數定義的順序，評估符合 `With` 方法參數的任何屬性。</span><span class="sxs-lookup"><span data-stu-id="1b113-149">The evaluation of any properties matching the `With` method parameters, in order of definition of the `With` method parameters.</span></span>