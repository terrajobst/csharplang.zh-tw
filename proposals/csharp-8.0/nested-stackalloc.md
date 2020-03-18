---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2019
ms.locfileid: "79485137"
---
# <a name="permit-stackalloc-in-nested-contexts"></a>允許在 nested 內容中 `stackalloc`

### <a name="stack-allocation"></a>堆疊配置

我們會修改C#語言規格的「[*堆疊配置*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation)」區段，以放寬 `stackalloc` 運算式可能出現時的地方。 我們刪除

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

並將其取代為

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

請注意，加入*stackalloc_initializer*的*array_initializer* （並讓索引運算式變成選擇性）是[7.3 中C#的延伸](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md)模組，此處並未說明。

`stackalloc` 運算式的*元素類型*是在 stackalloc 運算式中命名的*unmanaged_type* （如果有的話），或是在*array_initializer*的專案之間的一般類型，否則為。

*元素類型*`K` 的*stackalloc_initializer*類型取決於其語法內容：
- 如果*stackalloc_initializer*直接顯示為*local_variable_declaration*語句或*for_initializer*的*local_variable_initializer* ，則其類型會是 [`K*`]。
- 否則，它的類型會是 `System.Span<K>`。

### <a name="stackalloc-conversion"></a>Stackalloc 轉換

*Stackalloc 轉換*是從 expression 進行的新內建隱含轉換。 當*stackalloc_initializer*的型別 `K*`時，會有從*stackalloc_initializer*到 `System.Span<K>`類型的隱含*stackalloc 轉換*。
