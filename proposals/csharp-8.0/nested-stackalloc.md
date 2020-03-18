---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2019
ms.locfileid: "79485137"
---
# <a name="permit-stackalloc-in-nested-contexts"></a><span data-ttu-id="5ec89-101">允許在 nested 內容中 `stackalloc`</span><span class="sxs-lookup"><span data-stu-id="5ec89-101">Permit `stackalloc` in nested contexts</span></span>

### <a name="stack-allocation"></a><span data-ttu-id="5ec89-102">堆疊配置</span><span class="sxs-lookup"><span data-stu-id="5ec89-102">Stack allocation</span></span>

<span data-ttu-id="5ec89-103">我們會修改C#語言規格的「[*堆疊配置*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation)」區段，以放寬 `stackalloc` 運算式可能出現時的地方。</span><span class="sxs-lookup"><span data-stu-id="5ec89-103">We modify the section [*Stack allocation*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) of the C# language specification to relax the places when a `stackalloc` expression may appear.</span></span> <span data-ttu-id="5ec89-104">我們刪除</span><span class="sxs-lookup"><span data-stu-id="5ec89-104">We delete</span></span>

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="5ec89-105">並將其取代為</span><span class="sxs-lookup"><span data-stu-id="5ec89-105">and replace them with</span></span>

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

<span data-ttu-id="5ec89-106">請注意，加入*stackalloc_initializer*的*array_initializer* （並讓索引運算式變成選擇性）是[7.3 中C#的延伸](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md)模組，此處並未說明。</span><span class="sxs-lookup"><span data-stu-id="5ec89-106">Note that the addition of an *array_initializer* to *stackalloc_initializer* (and making the index expression optional) was an [extension in C# 7.3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) and is not described here.</span></span>

<span data-ttu-id="5ec89-107">`stackalloc` 運算式的*元素類型*是在 stackalloc 運算式中命名的*unmanaged_type* （如果有的話），或是在*array_initializer*的專案之間的一般類型，否則為。</span><span class="sxs-lookup"><span data-stu-id="5ec89-107">The *element type* of the `stackalloc` expression is the *unmanaged_type* named in the stackalloc expression, if any, or the common type among the elements of the *array_initializer* otherwise.</span></span>

<span data-ttu-id="5ec89-108">*元素類型*`K` 的*stackalloc_initializer*類型取決於其語法內容：</span><span class="sxs-lookup"><span data-stu-id="5ec89-108">The type of the *stackalloc_initializer* with *element type* `K` depends on its syntactic context:</span></span>
- <span data-ttu-id="5ec89-109">如果*stackalloc_initializer*直接顯示為*local_variable_declaration*語句或*for_initializer*的*local_variable_initializer* ，則其類型會是 [`K*`]。</span><span class="sxs-lookup"><span data-stu-id="5ec89-109">If the *stackalloc_initializer* appears directly as the *local_variable_initializer* of a *local_variable_declaration* statement or a *for_initializer*, then its type is `K*`.</span></span>
- <span data-ttu-id="5ec89-110">否則，它的類型會是 `System.Span<K>`。</span><span class="sxs-lookup"><span data-stu-id="5ec89-110">Otherwise its type is `System.Span<K>`.</span></span>

### <a name="stackalloc-conversion"></a><span data-ttu-id="5ec89-111">Stackalloc 轉換</span><span class="sxs-lookup"><span data-stu-id="5ec89-111">Stackalloc Conversion</span></span>

<span data-ttu-id="5ec89-112">*Stackalloc 轉換*是從 expression 進行的新內建隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="5ec89-112">The *stackalloc conversion* is a new built-in implicit conversion from expression.</span></span> <span data-ttu-id="5ec89-113">當*stackalloc_initializer*的型別 `K*`時，會有從*stackalloc_initializer*到 `System.Span<K>`類型的隱含*stackalloc 轉換*。</span><span class="sxs-lookup"><span data-stu-id="5ec89-113">When the type of a *stackalloc_initializer* is `K*`, there is an implicit *stackalloc conversion* from the *stackalloc_initializer* to the type `System.Span<K>`.</span></span>
