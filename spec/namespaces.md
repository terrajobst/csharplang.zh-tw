---
ms.openlocfilehash: 9c3863c9a139f5b8309fca6e0c099d0fae7677c3
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229594"
---
# <a name="namespaces"></a>命名空間

C# 程式組織方式使用命名空間。 命名空間用作為程式，「 內部 」 的組織系統和 「 外部 」 的組織系統 — 一種呈現會公開至其他程式的程式項目。

Using 指示詞 ([Using 指示詞](namespaces.md#using-directives)) 提供給方便使用的命名空間。

## <a name="compilation-units"></a>編譯單位

A *compilation_unit*定義原始程式檔的整體結構。 編譯單位包含零或多個*using_directive*s 後面接著零或多個*global_attributes*後面接著零或多個*namespace_member_declaration*s.

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

C# 程式包含了一個或多個編譯單位，每一個都包含不同的原始程式檔中。 C# 程式編譯時，所有的編譯單位都會一起處理。 因此，編譯單位可以彼此相依，可能是以循環方式。

*Using_directive*秒的編譯單位會影響*global_attributes*並*namespace_member_declaration*該編譯單位中，但有任何作用其他的編譯單位。

*Global_attributes* ([屬性](attributes.md)) 的編譯單位允許的目標組件和模組的屬性規格。 組件和模組做為型別的實體容器。 組件可能包含數個實際分開的模組。

*Namespace_member_declaration*之程式的每個編譯單位參與稱為全域命名空間的單一宣告空間的成員。 例如: 

檔案`A.cs`:
```csharp
class A {}
```

檔案`B.cs`:
```csharp
class B {}
```

兩個編譯單位參與單一全域命名空間，在此情況下宣告兩個類別的完整格式名稱具有`A`和`B`。 因為兩個編譯單位參與相同的宣告空間，就會發生錯誤如果每一個包含成員具有相同名稱的宣告。

## <a name="namespace-declarations"></a>命名空間宣告

A *namespace_declaration*組成關鍵字`namespace`，後面加上命名空間名稱和主體，或者後面接著一個分號。

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

A *namespace_declaration*中的最上層宣告可能會發生*compilation_unit*或在另一個成員宣告*namespace_declaration*。 當*namespace_declaration*中的最上層宣告，就會發生*compilation_unit*，命名空間會變成全域命名空間的成員。 當*namespace_declaration*就會發生在另一個*namespace_declaration*，內部的命名空間會變成外部命名空間的成員。 在任一情況下，必須是包含命名空間中的唯一命名空間名稱。

命名空間都是隱含`public`和命名空間宣告不能包含任何存取修飾詞。

內*namespace_body*，選擇性*using_directive*s 匯入其他命名空間、 類型和成員，以便直接而不是透過限定名稱所參考的名稱。 選擇性*namespace_member_declaration*s 提供的命名空間的宣告空間的成員。 請注意，所有*using_directive*s 必須出現在成員的任何宣告之前。

*Qualified_identifier*的*namespace_declaration*以單一識別項或分隔的識別項的序列可能是 「`.`"語彙基元。 第二個表單允許程式以定義巢狀命名空間，而不需要語彙巢狀數個命名空間宣告。 例如，套用至物件的

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
在語意上等於
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

命名空間是無限的但兩個具有相同的完整限定名稱的命名空間宣告參與相同的宣告空間 ([宣告](basic-concepts.md#declarations))。 在範例
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
上面的兩個命名空間宣告提供給相同的宣告空間，在此情況下宣告兩個類別的完整格式名稱具有`N1.N2.A`和`N1.N2.B`。 因為這兩種宣告參與相同的宣告空間，就會發生錯誤，如果每個都包含具有相同名稱的成員的宣告。

## <a name="extern-aliases"></a>外部別名

*Extern_alias_directive*引進識別項，做為命名空間的別名。 別名命名空間的規格是外部程式的原始程式碼，而且也適用於巢狀命名空間的別名命名空間。

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

範圍*extern_alias_directive*擴充*using_directive*s *global_attributes*並*namespace_member_declaration*立即包含編譯單位或命名空間主體的 s。

在編譯單位或命名空間主體中包含*extern_alias_directive*，所導入的識別項*extern_alias_directive*可用來參考別名命名空間。 它是編譯時期錯誤*識別碼*是 word `global`。

*Extern_alias_directive*讓別名可在特定的編譯單位或命名空間主體中，但它並沒有提供給基礎的宣告空間的任何新成員。 亦即*extern_alias_directive*不是可轉移的但相反地，會影響僅編譯單位或命名空間主體中發生。

下列程式會宣告並使用兩個外部別名`X`和`Y`，每個代表不同的命名空間階層的根的：
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

這個程式會宣告是否存在外部別名`X`和`Y`，但實際的別名定義是外部程式。 相同具名`N.B`類別現在可做為參考`X.N.B`並`Y.N.B`，或者，您也可以使用命名空間別名限定詞，`X::N.B`和`Y::N.B`。 如果程式宣告外部別名提供沒有外部的定義，就會發生錯誤。

## <a name="using-directives"></a>using 指示詞

***Using 指示詞***方便使用的命名空間和其他命名空間中定義的類型。 使用指示詞影響的名稱解析程序*namespace_or_type_name*s ([命名空間和類型名稱](basic-concepts.md#namespace-and-type-names)) 和*simple_name*s ([簡單名稱](expressions.md#simple-names))，但不同於 using 指示詞的宣告，並不會提供基礎的宣告空間的編譯單位或在其中使用的命名空間的新成員。

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

A *using_alias_directive* ([Using 別名指示詞](namespaces.md#using-alias-directives)) 導入了命名空間或類型的別名。

A *using_namespace_directive* ([Using 命名空間指示詞](namespaces.md#using-namespace-directives)) 匯入命名空間的型別成員。

A *using_static_directive* ([Using 靜態指示詞](namespaces.md#using-static-directives)) 匯入巢狀的類型和類型的靜態成員。

範圍*using_directive*擴充*namespace_member_declaration*立即包含編譯單位或命名空間主體的 s。 範圍*using_directive*特別不包含其對等*using_directive*s。 因此，對等互連*using_directive*s 並不會影響彼此，並在其中寫入的順序不重要。

### <a name="using-alias-directives"></a>Using 別名指示詞

A *using_alias_directive*導入做為別名的命名空間或型別，直接封入的編譯單位或命名空間主體中的識別項。

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

成員宣告中包含的編譯單位或命名空間主體*using_alias_directive*，所導入的識別項*using_alias_directive*可以用來參考指定命名空間或類型。 例如: 
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

上述成員宣告中`N3`命名空間`A`為其別名`N1.N2.A`，因此類別`N3.B`衍生自類別`N1.N2.A`。 可取得相同的效果，請建立別名`R`for `N1.N2` ，接著參考`R.A`:
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

*識別碼*的*using_alias_directive*編譯單位或立即包含命名空間的宣告空間內必須是唯一*using_alias_directive*. 例如: 
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

上述`N3`已包含的成員`A`，因此它是編譯時期錯誤*using_alias_directive*使用該識別項。 同樣地，它是兩個以上的編譯時期錯誤*using_alias_directive*相同編譯單位或命名空間主體內宣告相同名稱的別名。

A *using_alias_directive*讓別名可在特定的編譯單位或命名空間主體中，但它並沒有提供給基礎的宣告空間的任何新成員。 亦即*using_alias_directive*並非可轉移而是會影響僅編譯單位或命名空間主體中發生。 在範例
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
範圍*using_alias_directive*導入`R`只會延伸到其中它包含，命名空間內的成員宣告讓`R`不明第二個命名空間宣告中。 不過，放置*using_alias_directive*包含編譯單位會造成要供這兩個命名空間宣告中的別名：
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

就像一般的成員名稱則是所引進*using_alias_directive*s 會隱藏名稱相似的成員，在巢狀範圍中。 在範例
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
參考`R.A`中的宣告`B`造成編譯時期錯誤，因為`R`是指`N3.R`，而非`N1.N2`。

順序*using_alias_directive*s 會寫入不具有任何重要性，並解決*namespace_or_type_name*所參考*using_alias_directive*不會受到*using_alias_directive*本身或其他*using_directive*立即包含編譯單位或命名空間主體內。 亦即*namespace_or_type_name*的*using_alias_directive*如同立即包含的編譯單位或命名空間主體有不是解析*using_directive*s。 A *using_alias_directive*不過可能會受到*extern_alias_directive*立即包含編譯單位或命名空間主體內。 在範例
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
上次*using_alias_directive*導致編譯時期錯誤，因為它不會受到第一個*using_alias_directive*。 第一個*using_alias_directive*不會導致錯誤自外部別名的範圍`E`包括*using_alias_directive*。

A *using_alias_directive*可以建立任何命名空間或類型，包括在其中出現的命名空間的別名，而該命名空間內的任何命名空間或類型巢狀。

透過別名存取命名空間或類型，就會產生完全相同的結果做為透過它的宣告名稱存取該命名空間或類型。 例如，假設
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
名稱`N1.N2.A`， `R1.N2.A`，並`R2.A`對等項目和所有參考的類別完整的名稱是`N1.N2.A`。

使用別名時，可以封閉式建構類型的名稱，但無法命名為未繫結的泛型型別宣告未提供類型引數。 例如: 
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a>使用命名空間指示詞

A *using_namespace_directive*匯入到與其直接封入編譯單位或命名空間主體中，包含命名空間中啟用無限制地使用每個類型的識別項的型別。

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

成員宣告中包含的編譯單位或命名空間主體*using_namespace_directive*，可以直接參考包含在指定的命名空間中的型別。 例如: 
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

上述在成員宣告內`N3`命名空間、 類型成員`N1.N2`可直接使用，並因此類別`N3.B`衍生自類別`N1.N2.A`。

A *using_namespace_directive*匯入包含在指定的命名空間的類型，但特別是不會匯入巢狀命名空間。 在範例
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
*using_namespace_directive*中所包含的型別匯`N1`，但不是命名空間巢狀方式置於`N1`。 因此，若要參考`N2.A`中的宣告`B`導致編譯時期錯誤，因為沒有任何成員命名為`N2`範圍內。

不同於*using_alias_directive*，則*using_namespace_directive*可以匯入識別碼已在封入的編譯單位或命名空間主體中定義的類型。 作用中，名稱匯入*using_namespace_directive*隱藏名稱相似的成員，在封入的編譯單位或命名空間主體中。 例如: 
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

這裡，在成員宣告內`N3`命名空間`A`是指`N3.A`而非`N1.N2.A`。

當一個以上的命名空間或類型匯入*using_namespace_directive*s 或*using_static_directive*相同編譯單位或命名空間主體中包含相同名稱，也就是參考類型該名稱作為*type_name*會被視為模稜兩可。 在範例
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
兩者`N1`並`N2`包含成員`A`，而且因為`N3`匯入這兩者，參考`A`在`N3`是編譯時期錯誤。 在此情況下，衝突是可解析是透過參考限定`A`，或藉由引進*using_alias_directive* ，會挑選特定`A`。 例如: 
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

此外，當一個以上的命名空間或類型匯入*using_namespace_directive*s 或*using_static_directive*相同編譯單位或命名空間主體中包含類型或成員相同的名稱，該名稱參考*simple_name*會被視為模稜兩可。 在範例
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
`N1` 包含型別成員`A`，並`C`包含靜態方法`A`，而且因為`N2`匯入這兩者，參考`A`做為*simple_name*是模稜兩可和編譯時間發生錯誤。 

像是*using_alias_directive*，則*using_namespace_directive*並沒有提供任何新的成員，在基礎的宣告空間的編譯單位或命名空間，但而是只會影響編譯單位或命名空間主體中出現。

*Namespace_name*所參考*using_namespace_directive*解析為相同的方式*namespace_or_type_name*所參考*using_alias_directive*。 因此， *using_namespace_directive*相同編譯單位或命名空間主體中不會影響彼此，並可以依照任何順序來寫入。

### <a name="using-static-directives"></a>Using 靜態指示詞

A *using_static_directive*匯入巢狀型別和靜態成員直接包含在型別宣告成與其直接封入編譯單位或命名空間主體中，啟用 為每個成員和類型的識別碼使用無限制。

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

成員宣告中包含的編譯單位或命名空間主體*using_static_directive*，存取巢狀類型和靜態成員 （不包括擴充方法） 的宣告中直接包含指定的型別可以直接參考。 例如: 

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

上述成員宣告中`N2`命名空間的靜態成員和巢狀型別`N1.A`可以直接使用，因此方法`N`能夠參考`B`和`M`的成員`N1.A`.

A *using_static_directive*特別不匯入延伸模組方法，直接做為靜態的方法，但可讓這些擴充方法引動過程版本 ([擴充方法引動過程](expressions.md#extension-method-invocations))。 在範例

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
*using_static_directive*擴充方法會匯入`M`中所包含`N1.A`，但只作為擴充方法。 因此，第一次參考`M`中的主體`B.N`導致編譯時期錯誤，因為沒有任何成員命名為`M`範圍內。

A *using_static_directive*只會匯入成員和指定的型別，直接宣告的類型沒有成員和型別宣告基底類別中。

待辦事項：範例

多個之間的模稜兩可*using_namespace_directives*並*using_static_directives*討論[Using 命名空間指示詞](namespaces.md#using-namespace-directives)。

## <a name="namespace-members"></a>命名空間成員

A *namespace_member_declaration*是*namespace_declaration* ([命名空間宣告](namespaces.md#namespace-declarations)) 或*type_declaration* ([型別宣告](namespaces.md#type-declarations))。

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

編譯單位或命名空間主體可以包含*namespace_member_declaration*s 中，而這類宣告提供給基礎的宣告空間包含的編譯單位或命名空間主體的新成員。

## <a name="type-declarations"></a>型別宣告

A *type_declaration*是*class_declaration* ([類別宣告](classes.md#class-declarations))，則*struct_declaration* ([結構宣告](structs.md#struct-declarations))，則*interface_declaration* ([介面宣告](interfaces.md#interface-declarations))，則*enum_declaration* ([列舉宣告](enums.md#enum-declarations))，或有*delegate_declaration* ([委派宣告](delegates.md#delegate-declarations))。

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

A *type_declaration*為編譯單位的最上層宣告或命名空間、 類別或結構內宣告的成員可能會發生。

當型別宣告型別的`T`會顯示新宣告型別的完整的名稱就是為編譯單位的最上層宣告`T`。 當型別宣告型別的`T`就會發生的命名空間、 類別或結構的新宣告的型別完整名稱是`N.T`，其中`N`是包含命名空間、 類別或結構的完整的名稱。

在類別內宣告的型別或結構稱為巢狀型別 ([巢狀型別](classes.md#nested-types))。

允許的存取修飾詞和型別宣告的預設存取權取決於宣告進行的內容 ([宣告存取範圍](basic-concepts.md#declared-accessibility)):

*  在編譯單位或命名空間中宣告的型別可以有`public`或`internal`存取。 預設值是`internal`存取。
*  在類別中宣告的型別可以有`public`， `protected internal`， `protected`， `internal`，或`private`存取。 預設值是`private`存取。
*  在結構中宣告的型別可以有`public`， `internal`，或`private`存取。 預設值是`private`存取。

## <a name="namespace-alias-qualifiers"></a>命名空間別名限定詞

***命名空間別名限定詞***`::`讓您能夠保證型別名稱查閱會受到新的類型和成員的簡介。 命名空間別名限定詞永遠會出現稱為左側和右側的識別項的兩個識別碼之間。 不同於一般`.`限定詞，左側的識別項的`::`限定詞查詢最多只能為 extern 或使用別名。

A *qualified_alias_member*定義如下：

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

A *qualified_alias_member*可用來當做*namespace_or_type_name* ([命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)) 或中的左運算元為*member_access*([成員存取](expressions.md#member-access))。

A *qualified_alias_member*有兩種形式之一：

*  `N::I<A1, ..., Ak>`其中`N`並`I`表示識別項，以及`<A1, ..., Ak>`是型別引數清單。 (`K`總是至少一個。)
*  `N::I`其中`N`和`I`代表識別項。 (在此情況下，`K`則皆為零。)

使用這個標記法的意義*qualified_alias_member*判斷方式如下：

*  如果`N`是識別項`global`，然後搜尋全域命名空間`I`:
   * 如果全域命名空間包含名為命名空間 `I`並`K`為零，則*qualified_alias_member*指的是該命名空間。
   * 否則，如果全域命名空間包含名為非泛型型別 `I`並`K`為零，則*qualified_alias_member*參考該型別。
   * 否則，如果全域命名空間包含名為的型別 `I`具有`K` 型別參數，則*qualified_alias_member*建構具有指定的型別引數的型別參考。
   * 否則，請*qualified_alias_member*是未定義，而且會發生編譯時期錯誤。

*  否則，開頭的命名空間宣告 ([命名空間宣告](namespaces.md#namespace-declarations)) 立即包含*qualified_alias_member* （如果有的話），繼續使用每個封入的命名空間宣告（如果有的話），並包含編譯單位以作為結束*qualified_alias_member*，直到找到實體為止，會評估下列步驟：

   * 如果包含的命名空間宣告或編譯單位*using_alias_directive*建立關聯的`N`類型，則*qualified_alias_member*是未定義和編譯時間會發生錯誤。
   * 否則，如果命名空間宣告或編譯單位包含*extern_alias_directive*或是*using_alias_directive*建立關聯的`N`與命名空間，然後：
     * 如果命名空間相關聯`N`包含名為命名空間 `I`並`K`為零，則*qualified_alias_member*指的是該命名空間。
     * 否則，如果命名空間相關聯`N`包含名為非泛型型別 `I`並`K`為零，則*qualified_alias_member*參考該型別。
     * 否則，如果命名空間相關聯`N`包含名為的型別 `I`具有`K` 型別參數，則*qualified_alias_member*指的是以建構類型指定的型別引數。
     * 否則，請*qualified_alias_member*是未定義，而且會發生編譯時期錯誤。
*  否則，請*qualified_alias_member*是未定義，而且會發生編譯時期錯誤。

請注意，使用參考類型的別名的命名空間別名限定詞會使編譯時期錯誤。 也請注意，如果識別項 `N`已`global`，則即使沒有別名建立關聯的使用中全域命名空間中，執行查閱`global`與型別或命名空間。

### <a name="uniqueness-of-aliases"></a>別名的唯一性

每個編譯單位和命名空間主體具有個別的宣告空間，以讓外部別名和使用別名。 因此，雖然外部別名，或使用別名的名稱必須是唯一的外部別名集合中，並使用別名宣告直接包含的編譯單位或命名空間主體中，別名不允許有相同名稱的型別或命名空間，只要我只能搭配使用 t`::`限定詞。

在範例
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
名稱`A`第二個命名空間主體中有兩個可能的意義，因為這兩個類別`A`並使用別名`A`範圍內。 基於這個理由，利用`A`限定名稱中`A.Stream`模稜兩可，並會導致發生編譯時期錯誤。 不過，使用`A`具有`::`限定詞不是錯誤因為`A`查閱只會為命名空間別名。
