---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484696"
---
# <a name="conditional-ref-expressions"></a>條件式 ref 運算式

在中C#，有條件地將 ref 變數系結至一個或多個運算式的模式，目前無法表示。

一般的因應措施是引進如下的方法：

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

請注意，這不是三元的確切取代，因為所有引數都必須在呼叫位置進行評估。

下列作業將無法如預期般運作：

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

建議的語法如下所示：

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

您可以使用 ref 三元，_正確地_將上述嘗試寫入「選擇」，如下所示：

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

與選擇不同之處在于，結果和替代運算式會以_真正_的條件式方式存取，因此如果 ```arr == null```，就看不到損毀。

三元參考只是一個三元，其中的替代和結果都是 refs。 它自然會要求左值結果/替代運算元。 它也會要求結果和替代類型具有相互轉換的身分識別。

運算式的類型會以類似于一般三元的方式計算。 亦即. 在案例中，如果 [結果] 和 [替代] 具有可轉換的身分識別，但有不同的類型，則會套用現有的類型合併規則。

系統會從條件運算元中謹慎地假設安全回復。 如果其中一個不安全，則傳回的全部不安全。

Ref 三元是左值，因此可以透過傳址方式傳遞/指派/傳回;

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

做為左值，也可以指派給。 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

Ref 三元也可以用在一般（非 ref）內容中。 雖然它不是常見的，因為您也可以使用一般的三元。

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

執行附注： 

其實作的複雜性似乎是「中等對大」錯誤修正的大小。 -I. E 不昂貴。
我不認為我們需要對語法或剖析做任何變更。
不會影響中繼資料或 interop。 此功能是完全以運算式為基礎。
對調試/PDB 不會有任何影響
