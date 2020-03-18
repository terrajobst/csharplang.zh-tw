---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484696"
---
# <a name="conditional-ref-expressions"></a><span data-ttu-id="2695a-101">條件式 ref 運算式</span><span class="sxs-lookup"><span data-stu-id="2695a-101">Conditional ref expressions</span></span>

<span data-ttu-id="2695a-102">在中C#，有條件地將 ref 變數系結至一個或多個運算式的模式，目前無法表示。</span><span class="sxs-lookup"><span data-stu-id="2695a-102">The pattern of binding a ref variable to one or another expression conditionally is not currently expressible in C#.</span></span>

<span data-ttu-id="2695a-103">一般的因應措施是引進如下的方法：</span><span class="sxs-lookup"><span data-stu-id="2695a-103">The typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="2695a-104">請注意，這不是三元的確切取代，因為所有引數都必須在呼叫位置進行評估。</span><span class="sxs-lookup"><span data-stu-id="2695a-104">Note that this is not an exact replacement of a ternary since all arguments must be evaluated at the call site.</span></span>

<span data-ttu-id="2695a-105">下列作業將無法如預期般運作：</span><span class="sxs-lookup"><span data-stu-id="2695a-105">The following will not work as expected:</span></span>

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

<span data-ttu-id="2695a-106">建議的語法如下所示：</span><span class="sxs-lookup"><span data-stu-id="2695a-106">The proposed syntax would look like:</span></span>

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

<span data-ttu-id="2695a-107">您可以使用 ref 三元，_正確地_將上述嘗試寫入「選擇」，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2695a-107">The above attempt with "Choice" can be _correctly_ written using ref ternary as:</span></span>

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="2695a-108">與選擇不同之處在于，結果和替代運算式會以_真正_的條件式方式存取，因此如果 ```arr == null```，就看不到損毀。</span><span class="sxs-lookup"><span data-stu-id="2695a-108">The difference from Choice is that consequence and alternative expressions are accessed in a _truly_ conditional manner, so we do not see a crash if ```arr == null```</span></span>

<span data-ttu-id="2695a-109">三元參考只是一個三元，其中的替代和結果都是 refs。</span><span class="sxs-lookup"><span data-stu-id="2695a-109">The ternary ref is just a ternary where both alternative and consequence are refs.</span></span> <span data-ttu-id="2695a-110">它自然會要求左值結果/替代運算元。</span><span class="sxs-lookup"><span data-stu-id="2695a-110">It will naturally require that consequence/alternative operands are LValues.</span></span> <span data-ttu-id="2695a-111">它也會要求結果和替代類型具有相互轉換的身分識別。</span><span class="sxs-lookup"><span data-stu-id="2695a-111">It will also require that consequence and alternative have types that are identity convertible to each other.</span></span>

<span data-ttu-id="2695a-112">運算式的類型會以類似于一般三元的方式計算。</span><span class="sxs-lookup"><span data-stu-id="2695a-112">The type of the expression will be computed similarly to the one for the regular ternary.</span></span> <span data-ttu-id="2695a-113">亦即.</span><span class="sxs-lookup"><span data-stu-id="2695a-113">I.E.</span></span> <span data-ttu-id="2695a-114">在案例中，如果 [結果] 和 [替代] 具有可轉換的身分識別，但有不同的類型，則會套用現有的類型合併規則。</span><span class="sxs-lookup"><span data-stu-id="2695a-114">in a case if consequence and alternative have identity convertible, but different types, the existing type-merging rules will apply.</span></span>

<span data-ttu-id="2695a-115">系統會從條件運算元中謹慎地假設安全回復。</span><span class="sxs-lookup"><span data-stu-id="2695a-115">Safe-to-return will be assumed conservatively from the conditional operands.</span></span> <span data-ttu-id="2695a-116">如果其中一個不安全，則傳回的全部不安全。</span><span class="sxs-lookup"><span data-stu-id="2695a-116">If either is unsafe to return the whole thing is unsafe to return.</span></span>

<span data-ttu-id="2695a-117">Ref 三元是左值，因此可以透過傳址方式傳遞/指派/傳回;</span><span class="sxs-lookup"><span data-stu-id="2695a-117">Ref ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="2695a-118">做為左值，也可以指派給。</span><span class="sxs-lookup"><span data-stu-id="2695a-118">Being an LValue, it can also be assigned to.</span></span> 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

<span data-ttu-id="2695a-119">Ref 三元也可以用在一般（非 ref）內容中。</span><span class="sxs-lookup"><span data-stu-id="2695a-119">Ref ternary can be used in a regular (not ref) context as well.</span></span> <span data-ttu-id="2695a-120">雖然它不是常見的，因為您也可以使用一般的三元。</span><span class="sxs-lookup"><span data-stu-id="2695a-120">Although it would not be common since you could as well just use a regular ternary.</span></span>

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

<span data-ttu-id="2695a-121">執行附注：</span><span class="sxs-lookup"><span data-stu-id="2695a-121">Implementation notes:</span></span> 

<span data-ttu-id="2695a-122">其實作的複雜性似乎是「中等對大」錯誤修正的大小。</span><span class="sxs-lookup"><span data-stu-id="2695a-122">The complexity of the implementation would seem to be the size of a moderate-to-large bug fix.</span></span> <span data-ttu-id="2695a-123">-I. E 不昂貴。</span><span class="sxs-lookup"><span data-stu-id="2695a-123">- I.E not very expensive.</span></span>
<span data-ttu-id="2695a-124">我不認為我們需要對語法或剖析做任何變更。</span><span class="sxs-lookup"><span data-stu-id="2695a-124">I do not think we need any changes to the syntax or parsing.</span></span>
<span data-ttu-id="2695a-125">不會影響中繼資料或 interop。</span><span class="sxs-lookup"><span data-stu-id="2695a-125">There is no effect on metadata or interop.</span></span> <span data-ttu-id="2695a-126">此功能是完全以運算式為基礎。</span><span class="sxs-lookup"><span data-stu-id="2695a-126">The feature is completely expression based.</span></span>
<span data-ttu-id="2695a-127">對調試/PDB 不會有任何影響</span><span class="sxs-lookup"><span data-stu-id="2695a-127">No effect on debugging/PDB either</span></span>
