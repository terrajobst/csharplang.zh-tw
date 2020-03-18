---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/01/2020
ms.locfileid: "79485235"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="ee794-101">記錄進行中的工作</span><span class="sxs-lookup"><span data-stu-id="ee794-101">Records Work-in-Progress</span></span>

<span data-ttu-id="ee794-102">不同于其他記錄提案，這不是本身的提案，而是針對記錄功能記錄共識設計決策的工作進行中。</span><span class="sxs-lookup"><span data-stu-id="ee794-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="ee794-103">系統會視需要新增規格詳細資料以解決問題。</span><span class="sxs-lookup"><span data-stu-id="ee794-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="ee794-104">建議新增記錄的語法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ee794-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="ee794-105">`attributes` 的非終端機也會允許新的內容屬性，`data`。</span><span class="sxs-lookup"><span data-stu-id="ee794-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="ee794-106">使用參數清單或 `data` 修飾詞宣告的類別（結構）稱為記錄類別（記錄結構），其中之一是記錄類型。</span><span class="sxs-lookup"><span data-stu-id="ee794-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="ee794-107">宣告不含參數清單和 `data` 修飾詞的記錄類型是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="ee794-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="ee794-108">記錄類型的成員</span><span class="sxs-lookup"><span data-stu-id="ee794-108">Members of a record type</span></span>

<span data-ttu-id="ee794-109">除了在類別主體中宣告的成員之外，記錄類型還有下列其他成員：</span><span class="sxs-lookup"><span data-stu-id="ee794-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="ee794-110">主要的構造函式</span><span class="sxs-lookup"><span data-stu-id="ee794-110">Primary Constructor</span></span>

<span data-ttu-id="ee794-111">記錄類型具有公用的函式，其簽章對應于類型宣告的值參數。</span><span class="sxs-lookup"><span data-stu-id="ee794-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="ee794-112">這稱為類型的主要「函式」，並會隱藏隱含宣告的預設函數。</span><span class="sxs-lookup"><span data-stu-id="ee794-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="ee794-113">具有主要的函式，以及具有相同簽章的相同簽章已存在於類別中，是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="ee794-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="ee794-114">在執行時間，主要的函式</span><span class="sxs-lookup"><span data-stu-id="ee794-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="ee794-115">執行出現在類別主體中的實例欄位初始化運算式;然後叫用沒有引數的基類處理函式。</span><span class="sxs-lookup"><span data-stu-id="ee794-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="ee794-116">針對對應至值參數的屬性，初始化編譯器產生的支援欄位（如果這些屬性是由編譯器提供，請參閱[合成的屬性](#Synthesized Properties)）</span><span class="sxs-lookup"><span data-stu-id="ee794-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="ee794-117">[] TODO：新增基底呼叫語法和規格，以透過多載解析選擇基底函數</span><span class="sxs-lookup"><span data-stu-id="ee794-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="ee794-118">屬性</span><span class="sxs-lookup"><span data-stu-id="ee794-118">Properties</span></span>

<span data-ttu-id="ee794-119">對於記錄類型宣告的每個記錄參數，有一個對應的公用屬性成員，其名稱和類型取自值參數宣告。</span><span class="sxs-lookup"><span data-stu-id="ee794-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="ee794-120">如果沒有具有 get 存取子的具象（非抽象）屬性，而且已明確宣告或繼承此名稱和類型，則編譯器會產生，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ee794-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="ee794-121">若為記錄結構或記錄類別：</span><span class="sxs-lookup"><span data-stu-id="ee794-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="ee794-122">建立公用的僅限自動屬性。</span><span class="sxs-lookup"><span data-stu-id="ee794-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="ee794-123">它的值會在結構化期間使用對應的主要函式參數值進行初始化。</span><span class="sxs-lookup"><span data-stu-id="ee794-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="ee794-124">會覆寫每個「相符」的繼承抽象屬性 get 存取子。</span><span class="sxs-lookup"><span data-stu-id="ee794-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="ee794-125">等號比較成員</span><span class="sxs-lookup"><span data-stu-id="ee794-125">Equality members</span></span>

<span data-ttu-id="ee794-126">記錄類型會產生下列方法的合成實作為：</span><span class="sxs-lookup"><span data-stu-id="ee794-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="ee794-127">`object.GetHashCode()` 覆寫，除非已密封或使用者提供</span><span class="sxs-lookup"><span data-stu-id="ee794-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="ee794-128">`object.Equals(object)` 覆寫，除非已密封或使用者提供</span><span class="sxs-lookup"><span data-stu-id="ee794-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="ee794-129">`T Equals(T)` 方法，其中 `T` 為目前的類型</span><span class="sxs-lookup"><span data-stu-id="ee794-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="ee794-130">指定 `T Equals(T)` 以執行實值相等性，並比較每個主要的函式參數與另一個類型之對應屬性的名稱相同的屬性。</span><span class="sxs-lookup"><span data-stu-id="ee794-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="ee794-131">`object.Equals` 執行的對等</span><span class="sxs-lookup"><span data-stu-id="ee794-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```
