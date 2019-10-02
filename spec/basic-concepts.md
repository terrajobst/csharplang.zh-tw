---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704011"
---
# <a name="basic-concepts"></a>基本概念

## <a name="application-startup"></a>應用程式啟動

具有***進入點***的元件稱為「***應用程式***」。 當應用程式執行時，會建立新的***應用程式域***。 同一部電腦上可能同時存在數個不同的應用程式具現化，而且每個都有自己的應用程式域。

應用程式域會作為應用程式狀態的容器，藉此啟用應用程式隔離。 應用程式域會作為應用程式中所定義之型別的容器和界限，以及它所使用的類別庫。 載入至一個應用程式域的類型與載入另一個應用程式域中的相同類型不同，而且在應用程式域之間不會直接共用物件的實例。 例如，每個應用程式域都有自己的靜態變數複本，適用于這些類型，而且針對每個應用程式域，最多可執行一次類型的靜態函式。 建立和終結應用程式域的執行是免費的。

當執行環境呼叫指定的方法（稱為應用程式的進入點）時，就會***啟動應用程式***。 這個進入點方法一律會命名為 `Main`，而且可以有下列其中一個簽章：

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

如圖所示，進入點可能會選擇性地傳回 `int` 值。 此傳回值會用於應用程式終止（[應用程式終止](basic-concepts.md#application-termination)）。

進入點可能會選擇性地具有一個型式參數。 參數可能會有任何名稱，但參數的類型必須 `string[]`。 如果正式參數存在，執行環境會建立並傳遞 `string[]` 引數，其中包含啟動應用程式時所指定的命令列引數。 @No__t-0 引數永遠不會是 null，但如果未指定任何命令列引數，則其長度可能為零。

由於C#支援方法多載，因此類別或結構可能包含某個方法的多個定義，前提是每個都有不同的簽章。 不過，在單一程式中，沒有任何類別或結構可以包含一個以上的方法，稱為 `Main`，其定義會限定它做為應用程式進入點使用。 不過，如果其他多載版本的 `Main`，則會允許它們有一個以上的參數，或其唯一的參數不是類型 `string[]`。

應用程式可由多個類別或結構組成。 其中有多個類別或結構可以包含名為 `Main` 的方法，其定義會限定其做為應用程式進入點使用。 在這種情況下，必須使用外部機制（例如命令列編譯器選項）來選取其中一個 `Main` 方法做為進入點。

在C#中，每個方法都必須定義為類別或結構的成員。 一般來說，方法的宣告存取範圍（宣告[存取](basic-concepts.md#declared-accessibility)子）是由其宣告中指定的存取修飾詞（[存取](classes.md#access-modifiers)修飾詞）所決定，而類型的宣告存取範圍則是由在其宣告中指定的存取修飾詞。 為了讓給定類型的指定方法可供呼叫，類型和成員都必須是可存取的。 不過，應用程式進入點是特殊案例。 具體而言，執行環境可以存取應用程式的進入點，不論其已宣告的存取範圍為何，不論其封入類型宣告的宣告存取範圍為何。

應用程式進入點方法可能不在泛型類別宣告中。

在所有其他方面，進入點方法的行為就像不是進入點。

## <a name="application-termination"></a>應用程式終止

***應用程式終止***會將控制權傳回給執行環境。

如果應用程式***進入點***方法的傳回型別是 `int`，則傳回的值會作為應用程式的***終止狀態碼***。 此程式碼的目的是要允許成功或失敗到執行環境的通訊。

如果進入點方法的傳回型別為 `void`，而到達用來終止該方法的右大括弧（`}`），或執行沒有運算式的 @no__t 2 語句，則會產生終止狀態碼 `0`。

在應用程式終止之前，會呼叫其所有尚未進行垃圾收集之物件的析構函數，除非已隱藏這類清除（例如，藉由呼叫程式庫方法 `GC.SuppressFinalize`）。

## <a name="declarations"></a>宣告

C#程式中的宣告會定義程式的組成元素。 C#程式是使用命名空間（[命名](namespaces.md)空間）來組織，其中可以包含型別宣告和嵌套的命名空間宣告。 類型宣告（[類型](namespaces.md#type-declarations)宣告）是用來定義類別（[類別](classes.md)）、結構（[結構](structs.md)）、介面（[介面](interfaces.md)）、[列舉（列舉](enums.md)）和委派（[委派](delegates.md)）。 類型宣告中允許的成員類型，取決於類型宣告的形式。 例如，類別宣告可以包含常數（[常數](classes.md#constants)）、欄位（[欄位](classes.md#fields)）、方法（[方法](classes.md#methods)）、屬性（[屬性](classes.md#properties)）、事件（[事件](classes.md#events)）、索引子（[索引子](classes.md#indexers)）的宣告、運算子（[運算子](classes.md#operators)）、實例的構造函式（[實例](classes.md#instance-constructors)的函式）、靜態的函式（[靜態](classes.md#static-constructors)的函式）、析構函式（[析構](classes.md#destructors)函式）和巢狀型別（[巢狀型別](classes.md#nested-types)）。

宣告會在宣告所屬的宣告***空間***中定義名稱。 除了多載成員（簽章[和](basic-concepts.md#signatures-and-overloading)多載）以外，有兩個或多個宣告會在宣告空間中引進具有相同名稱的成員，這是編譯時期錯誤。 宣告空間永遠不可能包含具有相同名稱的不同類型成員。 例如，宣告空間絕不能包含具有相同名稱的欄位和方法。

有數種不同類型的宣告空間，如下所述。

*  在程式的所有原始程式檔中，沒有封入*namespace_declaration*的*namespace_member_declaration*不是單一合併宣告空間的成員，稱為***全域宣告空間***。
*  在程式的所有原始程式檔中，在具有相同完整命名空間名稱的*namespace_declaration*內， *namespace_member_declaration*s 都是單一合併宣告空間的成員。
*  每個類別、結構或介面宣告都會建立新的宣告空間。 名稱會透過*class_member_declaration*s、 *struct_member_declaration*s、 *interface_member_declaration*s 或*type_parameter*s 引進此宣告空間。 除了多載實例的「函式宣告」和「靜態」的「函式宣告」以外，類別或結構不能包含與類別或結構同名的成員宣告。 類別、結構或介面允許多載方法和索引子的宣告。 此外，類別或結構允許宣告多載實例的函式和運算子。 例如，類別、結構或介面可能包含多個具有相同名稱的方法宣告，但前提是這些方法宣告在其簽章（簽章[和](basic-concepts.md#signatures-and-overloading)多載）中不同。 請注意，基類不會參與類別的宣告空間，而且基底介面不會參與介面的宣告空間。 因此，可以使用衍生類別或介面，宣告與繼承成員同名的成員。 這類成員稱為***隱藏***繼承的成員。
*  每個委派宣告都會建立新的宣告空間。 名稱會透過型式參數（*fixed_parameter*s 和*parameter_array*s）和*type_parameter*，引進此宣告空間。
*  每個列舉宣告都會建立新的宣告空間。 名稱會透過*enum_member_declarations*引進到此宣告空間中。
*  每個方法宣告、索引子宣告、運算子宣告、實例的「函式宣告」和「匿名函式」都會建立新的宣告空間，稱為***本機變數宣告空間***。 名稱會透過型式參數（*fixed_parameter*s 和*parameter_array*s）和*type_parameter*，引進此宣告空間。 函式成員或匿名函式的主體（如果有的話）會被視為在區域變數宣告空間內嵌套。 區域變數宣告空間和嵌套的區域變數宣告空間會包含相同名稱的元素，這是錯誤。 因此，在嵌套的宣告空間內，不可能在封閉宣告空間中宣告與區域變數或常數同名的區域變數或常數。 兩個宣告空格可以包含具有相同名稱的專案，但前提是兩者的宣告空間都不包含另一個。
*  每個*區塊*或*switch_block* ，以及*for*、 *foreach*和*using*語句都會針對本機變數和本機常數建立區域變數宣告空間。 名稱會透過*local_variable_declaration*s 和*local_constant_declaration*s 引進此宣告空間。 請注意，在函式成員或匿名函式的主體中發生的區塊，會嵌套在這些函式所宣告的區域變數宣告空間中，以做為其參數。 因此，有一個錯誤，例如具有本機變數的方法和具有相同名稱的參數。
*  每個*區塊*或*switch_block*會為標籤建立個別的宣告空間。 名稱會透過*labeled_statement*在此宣告空間中引進，而名稱則會透過*goto_statement*來參考。 區塊的***標籤宣告空間***包含任何嵌套區塊。 因此，在嵌套區塊內，不可能宣告與封閉區塊中的標籤同名的標籤。

宣告名稱的文字順序通常沒有任何意義。 特別的是，文字順序對於宣告和使用命名空間、常數、方法、屬性、事件、索引子、運算子、實例函式、析構函式、靜態函式和類型而言並不重要。 宣告順序在下列方面很重要：

*  欄位宣告和區域變數宣告的宣告順序會決定其初始化運算式（如果有的話）的執行順序。
*  您必須先定義區域變數，才能使用這些變數（[範圍](basic-concepts.md#scopes)）。
*  省略*constant_expression*值時，列舉成員宣告（[列舉成員](enums.md#enum-members)）的宣告順序是很重要的。

命名空間的宣告空間是「開放式結束」，而兩個具有相同完整名稱的命名空間宣告會參與相同的宣告空間。 例如：
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

上述兩個命名空間宣告會構成相同的宣告空間，在此案例中，宣告兩個具有完整名稱 `Megacorp.Data.Customer` 和 `Megacorp.Data.Order` 的類別。 由於這兩個宣告會構成相同的宣告空間，因此如果每個宣告都包含具有相同名稱的類別宣告，則會導致編譯時期錯誤。

如上面所指定，區塊的宣告空間包含任何嵌套區塊。 因此，在下列範例中，`F` 和 @no__t 1 方法會導致編譯時期錯誤，因為名稱 `i` 是在外部區塊中宣告，而且不能在內部區塊中重新宣告。 不過，`H` 和 @no__t 1 方法有效，因為兩個 `i` 會在個別的非嵌套區塊中宣告。

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a>成員

命名空間和類型具有***成員***。 實體的成員通常是透過使用以實體的參考開頭的限定名稱，後面接著 "`.`" token，再加上成員名稱來提供。

類型的成員可以在類型宣告中宣告，或***繼承***自類型的基類。 當型別繼承自基類時，基類的所有成員（實例函式除外、析構函式和靜態的函式）都會變成衍生型別的成員。 基類成員的宣告存取範圍不會控制是否繼承成員，而繼承會延伸至不是實例的函式、靜態的函數或析構函數的任何成員。 不過，繼承的成員可能無法在衍生型別中存取，因為它已宣告的存取範圍（宣告的[存取](basic-concepts.md#declared-accessibility)範圍），或因為型別本身的宣告隱藏了（[透過繼承隱藏](basic-concepts.md#hiding-through-inheritance)）。

### <a name="namespace-members"></a>命名空間成員

沒有封閉式命名空間的命名空間和類型是***全域命名空間***的成員。 這會直接對應到全域宣告空間中宣告的名稱。

命名空間中宣告的命名空間和類型是該命名空間的成員。 這會直接對應至命名空間宣告空間中宣告的名稱。

命名空間沒有存取限制。 您不能宣告私用、受保護的或內部命名空間，而且命名空間名稱一律可公開存取。

### <a name="struct-members"></a>結構成員

結構的成員是在結構中宣告的成員，以及從結構的直接基類繼承而來的成員 `System.ValueType` 和間接基類 `object`。

簡單類型的成員會直接對應至簡單類型所別名之結構類型的成員：

*  @No__t-0 的成員是 `System.SByte` 結構的成員。
*  @No__t-0 的成員是 `System.Byte` 結構的成員。
*  @No__t-0 的成員是 `System.Int16` 結構的成員。
*  @No__t-0 的成員是 `System.UInt16` 結構的成員。
*  @No__t-0 的成員是 `System.Int32` 結構的成員。
*  @No__t-0 的成員是 `System.UInt32` 結構的成員。
*  @No__t-0 的成員是 `System.Int64` 結構的成員。
*  @No__t-0 的成員是 `System.UInt64` 結構的成員。
*  @No__t-0 的成員是 `System.Char` 結構的成員。
*  @No__t-0 的成員是 `System.Single` 結構的成員。
*  @No__t-0 的成員是 `System.Double` 結構的成員。
*  @No__t-0 的成員是 `System.Decimal` 結構的成員。
*  @No__t-0 的成員是 `System.Boolean` 結構的成員。

### <a name="enumeration-members"></a>列舉成員

列舉的成員是列舉型別中所宣告的常數，以及從列舉的直接基類繼承而來的成員 `System.Enum` 和間接基類 `System.ValueType` 和 `object`。

### <a name="class-members"></a>類別成員

類別的成員是在類別中宣告的成員，以及繼承自基類的成員（除了沒有基類的類別 `object` 除外）。 繼承自基類的成員包含常數、欄位、方法、屬性、事件、索引子、運算子和基類的類型，但不包括實例的函式、析構函式和基類的靜態函式。 繼承基類成員，而不考慮其存取範圍。

類別宣告可能包含常數、欄位、方法、屬性、事件、索引子、運算子、實例函式、析構函數、靜態函式和類型的宣告。

@No__t-0 和 `string` 的成員直接對應至其別名的類別類型成員：

*  @No__t-0 的成員是 `System.Object` 類別的成員。
*  @No__t-0 的成員是 `System.String` 類別的成員。

### <a name="interface-members"></a>介面成員

介面的成員是介面中宣告的成員，以及介面的所有基底介面。 類別中的成員 `object` 不是嚴格的說，就是任何介面（[介面成員](interfaces.md#interface-members)）的成員。 不過，可以透過任何介面類別型（[成員查閱](expressions.md#member-lookup)）中的成員查閱，取得類別中 `object` 的成員。

### <a name="array-members"></a>陣列成員

陣列的成員是繼承自類別 `System.Array` 的成員。

### <a name="delegate-members"></a>委派成員

委派的成員是繼承自類別 `System.Delegate` 的成員。

## <a name="member-access"></a>成員存取

成員的宣告允許對成員存取進行控制。 成員的存取範圍是由成員的宣告存取範圍（宣告的[存取](basic-concepts.md#declared-accessibility)範圍）所建立，並結合立即包含類型的存取範圍（如果有的話）。

允許存取特定成員時，該成員會被視為可***存取***。 相反地，如果不允許存取特定成員，則會將該***成員視為無法存取。*** 當存取進行的文字位置包含在成員的存取範圍定義域（[存取](basic-concepts.md#accessibility-domains)範圍網域）中時，允許存取成員。

### <a name="declared-accessibility"></a>已宣告存取範圍

成員的宣告***存取***範圍可以是下列其中一項：

*  Public，其選取方式是在成員宣告中包含 `public` 修飾詞。 @No__t-0 的直覺意義是「存取不受限制」。
*  Protected，藉由在成員宣告中包含 `protected` 修飾詞來選取。 @No__t-0 的直覺意義是「存取限於包含類別或衍生自包含類別的類型」。
*  內部：在成員宣告中包含 `internal` 修飾詞，以選取此選項。 @No__t-0 的直覺意義是「僅限存取此程式」。
*  受保護的內部（表示 protected 或 internal），其選取方式是在成員宣告中包含 `protected` 和 @no__t 1 修飾詞。 @No__t-0 的直覺意義是「存取限制為此程式或衍生自包含類別的類型」。
*  私用，藉由在成員宣告中包含 `private` 修飾詞來選取。 @No__t-0 的直覺意義是「存取限制為包含的類型」。

視成員宣告發生的內容而定，只允許特定類型的已宣告存取範圍。 此外，當成員宣告不包含任何存取修飾詞時，宣告所處的內容會決定預設宣告的存取範圍。

*  命名空間隱含具有 `public` 宣告的存取範圍。 命名空間宣告上不允許有任何存取修飾詞。
*  在編譯單位或命名空間中宣告的類型，可以有 `public` 或 @no__t 1 宣告的存取範圍，而且預設為 @no__t 2 宣告的存取範圍。
*  類別成員可以具有五種宣告存取範圍的任何一種，而且預設為 @no__t 0 宣告的存取範圍。 （請注意，宣告為類別成員的型別可以擁有任何五種宣告的存取範圍，而宣告為命名空間成員的型別只能 `public` 或 @no__t 1 宣告的存取範圍）。
*  結構成員可以有 `public`、`internal` 或 @no__t 2 宣告的存取範圍，而且預設為 @no__t 3 宣告的存取範圍，因為結構是隱含密封的。 結構中引進的結構成員（也就是，不是由該結構繼承）不能有 `protected` 或 @no__t 1 宣告的存取範圍。 （請注意，宣告為結構成員的型別可以有 `public`、`internal` 或 @no__t 2 宣告的存取範圍，而宣告為命名空間成員的型別只能有 `public` 或 `internal` 宣告的存取範圍）。
*  介面成員隱含具有 `public` 宣告的存取範圍。 介面成員宣告上不允許有任何存取修飾詞。
*  列舉成員隱含具有 `public` 宣告的存取範圍。 列舉成員宣告上不允許有任何存取修飾詞。

### <a name="accessibility-domains"></a>存取範圍網域

成員的***存取範圍定義域***是由允許存取成員的程式文字（可能不相鄰）區段所組成。 為了定義成員的存取範圍定義域，如果成員未在型別中宣告，則會將其視為***最上層***，而如果成員是在另一個型別內宣告，則會將其視為***嵌套***。 此外，程式的***程式文字***會定義為程式的所有原始程式檔中包含的所有程式文字，而類型的程式文字則定義為該類型之*type_declaration*中包含的所有程式文字（包括、可能是在類型中嵌套的類型）。

預先定義類型的存取範圍定義域（例如 `object`、`int` 或 `double`）是無限制的。

在程式 `P` 中宣告之最上層未系結類型 `T` （系結[和未](types.md#bound-and-unbound-types)系結類型）的存取範圍定義域定義如下：

*  如果 `T` 的宣告存取範圍是 `public`，`T` 的存取範圍定義域就是 `P` 的程式文字，以及參考 `P` 的任何程式。
*  如果 `T` 的宣告存取範圍是 `internal`，`T` 的存取範圍領域就是 `P` 的程式文字。

從這些定義來看，最上層未系結類型的存取範圍定義域一律至少是宣告該類型之程式的程式文字。

結構化類型的存取範圍定義域 `T<A1, ..., An>` 是未系結泛型型別的存取範圍定義域的交集 `T` 以及類型引數 `A1, ..., An` 的存取範圍定義域。

在程式 `P` 的 @no__t 類型中宣告的嵌套成員 `M` 的存取範圍定義域，如下所示（請注意，`M` 本身可能是類型）：

*  如果 `M` 的宣告存取範圍是 `public`，`M` 的存取範圍領域就是 `T` 的存取範圍領域。
*  如果 `M` 的宣告存取範圍是 `protected internal`，請讓 `D` 成為 `P` 的程式文字和任何衍生自 `T` 之類型的程式文字的聯集，而這是在 `P` 外部宣告。 @No__t-0 的存取範圍定義域是 `T` 的存取範圍網域與 `D` 的交集。
*  如果 `M` 的宣告存取範圍是 `protected`，請讓 `D` 成為 `T` 的程式文字和任何衍生自 `T` 之類型的程式文字的聯集。 @No__t-0 的存取範圍定義域是 `T` 的存取範圍網域與 `D` 的交集。
*  如果 `M` 的宣告存取範圍是 `internal`，`M` 的存取範圍領域是 `T` 的存取範圍領域與 `P` 的程式文字的交集。
*  如果 `M` 的宣告存取範圍是 `private`，`M` 的存取範圍領域就是 `T` 的程式文字。

從這些定義來看，嵌套成員的存取範圍定義域一律至少是宣告該成員之類型的程式文字。 此外，它還會遵循成員的存取範圍定義域，而不會比宣告該成員之類型的存取範圍定義域更包含在內。

以直覺的角度來說，當存取類型或成員 `M` 時，會評估下列步驟，以確保允許存取：

*  首先，如果在型別（相對於編譯單位或命名空間）內宣告 `M`，則如果無法存取該類型，就會發生編譯時期錯誤。
*  然後，如果 `M` `public`，則允許存取。
*  否則，如果 `M` 是 `protected internal`，則允許存取（如果是在宣告 `M` 的程式中），或是發生在衍生自類別的類別中（其中已宣告 `M`，並會透過衍生類別類型進行）（[Protected實例成員的存取權](basic-concepts.md#protected-access-for-instance-members)）。
*  否則，如果 `M` 是 `protected`，則如果在宣告 `M` 的類別內，或在衍生自類別的類別（其中已宣告了 `M`，並透過衍生類別類型執行）中，就會允許存取（[Protected實例成員的存取權](basic-concepts.md#protected-access-for-instance-members)）。
*  否則，如果 `M` 是 `internal`，則允許在宣告 `M` 的程式中進行存取。
*  否則，如果 `M` 是 `private`，則如果在宣告 `M` 的類型內發生，則允許存取。
*  否則，無法存取類型或成員，而且會發生編譯時期錯誤。

在範例中
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
類別和成員具有下列存取範圍網域：

*  @No__t-0 和 `A.X` 的存取範圍網域不受限制。
*  @No__t-0、`B`、`B.X`、`B.Y`、`B.C`、`B.C.X` 和 `B.C.Y` 的存取範圍定義域是包含程式的程式文字。
*  @No__t-0 的存取範圍網域是 `A` 的程式文字。
*  @No__t-0 的存取範圍網域和 `B.D` 是 `B` 的程式文字，包括 `B.C` 和 `B.D` 的程式文字。
*  @No__t-0 的存取範圍網域是 `B.C` 的程式文字。
*  @No__t-0 的存取範圍網域和 `B.D.Y` 是 `B` 的程式文字，包括 `B.C` 和 `B.D` 的程式文字。
*  @No__t-0 的存取範圍網域是 `B.D` 的程式文字。

如範例所示，成員的存取範圍定義域絕不會大於包含類型的存取範圍網域。 例如，即使所有 @no__t 0 的成員都具有公用的已宣告存取範圍，但 `A.X` 則擁有受限於包含類型的存取範圍網域。

如[成員](basic-concepts.md#members)中所述，衍生類型會繼承基類的所有成員，但實例的函式、析構函式和靜態的函式除外。 這也包括基類的私用成員。 不過，私用成員的存取範圍定義域只會包含宣告該成員之類型的程式文字。 在範例中
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
@no__t 0 類別會從 @no__t 2 類別繼承 `x` 的私用成員。 因為此成員是私用的，所以只能在 `A` 的*class_body*中存取。 因此，`b.x` 的存取會成功在 `A.F` 方法中，但會在 @no__t 2 方法中失敗。

### <a name="protected-access-for-instance-members"></a>實例成員的受保護存取

當 `protected` 實例成員在宣告它的類別的程式文字之外存取時，以及在宣告的程式文字外部存取 @no__t 1 實例成員時，必須在類別內進行存取。衍生自其宣告所在之類別的宣告。 此外，您必須透過衍生類別型別的實例或從它所構成的類別型別，來進行存取。 這項限制可防止一個衍生類別存取其他衍生類別的受保護成員，即使成員繼承自相同的基類也一樣。

Let `B` 是宣告受保護實例成員 `M` 的基類，讓 `D` 是衍生自 `B` 的類別。 在 `D` 的*class_body*中，`M` 的存取權可以採用下列其中一種格式：

*  不合格的*type_name*或格式為 `M` 的*primary_expression* 。
*  @No__t-1 格式的*primary_expression* ，前提是 `E` 的類型為 `T`，或衍生自 `T` 的類別，其中 `T` 是類別類型 `D`，或從 `D` 所構成的類別類型
*  @No__t-1 格式的*primary_expression* 。

除了這些存取形式，衍生的類別也可以在*constructor_initializer*中存取基類的受保護實例（「函式[初始化運算式](classes.md#constructor-initializers)」）。

在範例中
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
在 `A` 中，可以透過 `A` 和 `B` 的實例存取 `x`，因為在任一情況下，都會透過 `A` 的實例或從 `A` 衍生的類別來進行存取。 不過，在 `B` 中，無法透過 `A` 的實例存取 `x`，因為 `A` 不是從 `B` 衍生而來。

在範例中
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
允許 `x` 的三個指派，因為它們都是透過從泛型型別所建立的類別類型實例進行。

### <a name="accessibility-constraints"></a>協助工具條件約束

在C#語言中，數個結構的類型必須至少可以如同成員或其他類型***一樣存取***。 如果 `T` 的存取範圍定義域是 `M` 的存取範圍領域的超集合，則 `T` 的類型會被視為至少可以如同成員或類型 `M` 一樣存取。 換句話說，如果在可存取 `M` 的所有內容中，`T` 可供存取，則 `T` 至少會如 `M` 一樣存取。

存在下列協助工具條件約束：

*  類別類型的直接基底類別至少必須可以像類別類型本身一樣地存取。
*  介面類型的明確基底介面至少必須可以像介面類型本身一樣地存取。
*  委派類型的傳回類型和參數類型至少必須可以像委派類型本身一樣地存取。
*  常數的類型至少必須可以像常數本身一樣地存取。
*  欄位的類型至少必須可以像欄位本身一樣地存取。
*  方法的傳回類型和參數類型至少必須可以像方法本身一樣地存取。
*  屬性的類型至少必須可以像屬性本身一樣地存取。
*  事件的類型至少必須可以像事件本身一樣地存取。
*  索引子的類型和參數類型至少必須可以像索引子本身一樣地存取。
*  運算子的傳回類型和參數類型至少必須可以像運算子本身一樣地存取。
*  實例構造函式的參數類型必須至少與實例的函式本身一樣可以存取。

在範例中
```csharp
class A {...}

public class B: A {...}
```
@no__t 0 類別會導致編譯時期錯誤，因為 `A` 的存取權至少不如 `B`。

同樣地，在此範例中
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
`B` 中的 `H` 方法會導致編譯時期錯誤，因為傳回的類型 `A` 不是至少與方法相同的存取權。

## <a name="signatures-and-overloading"></a>簽章和多載

方法、實例的函式、索引子和運算子都是***以其簽章為特徵：***

*  方法的簽章包含方法的名稱、型別參數的數目，以及每一個型式參數的類型和種類（值、參考或輸出），以從左至右的順序來考慮。 基於這些目的，在型式參數類型中發生之方法的任何型別參數，都不是以其名稱來識別，而是依其在方法的型別引數清單中的序數位置。 方法的簽章特別不包含傳回型別、可為最右邊參數指定的 `params` 修飾詞，也不包括選擇性的型別參數條件約束。
*  實例函式的簽章包含其每一個型式參數的類型和種類（值、參考或輸出），並以從左至右的順序來考慮。 實例函式的簽章特別不包含可為最右邊參數指定的 `params` 修飾詞。
*  索引子的簽章是由它的每個型式參數的類型所組成，並以由左至右的順序來表示。 索引子的簽章特別不包含元素類型，也不包含可為最右邊參數指定的 `params` 修飾詞。
*  運算子的簽章是由運算子的名稱和每個型式參數的類型所組成，並以從左至右的順序來考慮。 運算子的簽章特別不包含結果型別。

簽章是在類別、結構和介面中，多載***成員的啟用***機制：

*  方法的多載可讓類別、結構或介面宣告多個具有相同名稱的方法，但前提是其簽章在該類別、結構或介面中是唯一的。
*  如果類別或結構的簽章在該類別或結構內是唯一的，則實例函式的多載可讓您宣告多個實例的函式。
*  索引子的多載可讓類別、結構或介面宣告多個索引子，但前提是其簽章在該類別、結構或介面中是唯一的。
*  運算子的多載可讓類別或結構宣告多個具有相同名稱的運算子，但前提是其簽章在該類別或結構中是唯一的。

雖然 `out` 和 @no__t 1 參數修飾詞是簽章的一部分，但在單一類型中宣告的成員，在簽章中不能單獨由 `ref` 和 `out` 而有所不同。 如果兩個具有 `out` 修飾詞的方法中的所有參數都已變更為 `ref` 修飾詞，則在相同類型中使用相同的簽章宣告兩個成員時，就會發生編譯時期錯誤。 針對簽章比對的其他用途（例如，隱藏或覆寫），`ref`，而 `out` 則視為簽章的一部分，而且彼此不相符。 （這項限制是為了C#讓程式能夠輕鬆地轉譯成在通用語言基礎結構（CLI）上執行，這不會提供方法來定義只在 `ref` 和 `out` 中不同的方法）。

基於簽章的目的，`object` 和 `dynamic` 的類型會被視為相同。 因此，在單一類型中宣告的成員，在簽章中可能不會單獨受到 `object` 和 `dynamic` 的差異。

下列範例會顯示一組多載的方法宣告以及其簽章。
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

請注意，任何 `ref` 和 @no__t 1 參數修飾詞（[方法參數](classes.md#method-parameters)）都是簽章的一部分。 因此，`F(int)` 和 `F(ref int)` 是唯一的簽章。 不過，不能在相同的介面內宣告 `F(ref int)` 和 `F(out int)`，因為它們的簽章不同于 `ref` 和 `out`。 另請注意，傳回型別和 `params` 修飾詞不是簽章的一部分，因此不可能根據傳回型別或包含或排除 `params` 修飾詞而單獨進行多載。 因此，上述的方法宣告 `F(int)`，而 `F(params string[])` 則會導致編譯時期錯誤。

## <a name="scopes"></a>範圍

名稱的***範圍***是程式文字的區域，可以在其中參考名稱所宣告的實體，而不需要限定名稱。 範圍可以是***嵌套***的，而且內部範圍可能會從外部範圍重新宣告名稱的意義（不過，這不會移除嵌套區塊內的宣告所[加諸](basic-concepts.md#declarations)的限制，因此無法使用與封閉區塊中的本機變數同名）。 外部範圍中的名稱會被視為***隱藏***于內部範圍所涵蓋的程式文字區域中，而且只有限定名稱才能存取外部名稱。

*  *Namespace_member_declaration* （[命名空間成員](namespaces.md#namespace-members)）所宣告的命名空間成員範圍（不含封閉的*namespace_declaration* ）是整個程式文字。
*  *Namespace_declaration*內的*namespace_member_declaration*所宣告的命名空間成員範圍，其完整名稱為 `N` 是每個*namespace_declaration*的*namespace_body* ，其完整限定名稱為 `N`，或以 `N` 開頭，後面接著句號。
*  *Extern_alias_directive*所定義的名稱範圍會延伸到其立即包含編譯單位或命名空間主體的*using_directive*s、 *global_attributes*和*namespace_member_declaration*s。 *Extern_alias_directive*不會對基礎宣告空間提供任何新成員。 換句話說， *extern_alias_directive*不是可轉移的，而是只會影響其發生所在的編譯單位或命名空間主體。
*  *Using_directive* （[using](namespaces.md#using-directives)指示詞）所定義或匯入之名稱的範圍會延伸到*compilation_unit*或*namespace_body*的*namespace_member_declaration*s，其中*using_directive*發生。 *Using_directive*可能會在特定的*compilation_unit*或*namespace_body*內提供零個或多個命名空間、類型或成員名稱，但不會將任何新成員貢獻給基礎宣告空間。 換句話說， *using_directive*無法轉移，而只會影響其發生的*compilation_unit*或*namespace_body* 。
*  *Class_declaration* （[類別](classes.md#class-declarations)宣告）上*type_parameter_list*所宣告之類型參數的範圍是該的*class_base*、 *type_parameter_constraints_clause*s 和*class_body* *class_declaration*。
*  *Struct_declaration*上的*type_parameter_list*所宣告之類型參數的範圍（[結構](structs.md#struct-declarations)宣告）為*struct_interfaces*、 *type_parameter_constraints_clause*s 和*struct_body* ，屬於這*struct_declaration*。
*  *Interface_declaration* （[介面](interfaces.md#interface-declarations)宣告）上*type_parameter_list*所宣告之類型參數的範圍為*interface_base*、 *type_parameter_constraints_clause*s 和*interface_body*的*interface_declaration*。
*  *Delegate_declaration* （[委派](delegates.md#delegate-declarations)宣告）上*type_parameter_list*所宣告之類型參數的範圍為*return_type*、 *formal_parameter_list*和*type_parameter_constraints_clause* *delegate_declaration*。
*  *Class_member_declaration* （[類別主體](classes.md#class-body)）所宣告的成員範圍是宣告發生所在的*class_body* 。 此外，類別成員的範圍會延伸至成員的存取範圍定義域（[存取範圍定義域](basic-concepts.md#accessibility-domains)）中所包含的衍生類別的*class_body* 。
*  *Struct_member_declaration* （[結構成員](structs.md#struct-members)）所宣告的成員範圍是宣告發生所在的*struct_body* 。
*  *Enum_member_declaration* （[列舉成員](enums.md#enum-members)）所宣告的成員範圍是宣告發生所在的*enum_body* 。
*  在*method_declaration* （[方法](classes.md#methods)）中宣告的參數範圍是該*method_declaration*的*method_body* 。
*  在*indexer_declaration* （[索引子](classes.md#indexers)）中宣告的參數範圍是該*indexer_declaration*的*accessor_declarations* 。
*  在*operator_declaration* （[運算子](classes.md#operators)）中宣告的參數範圍是該*operator_declaration*的*區塊*。
*  在*constructor_declaration*中宣告的參數範圍（[實例](classes.md#instance-constructors)的程式，也就是該*constructor_declaration*的*constructor_initializer*和*區塊*）。
*  在*lambda_expression*中宣告的參數範圍（[匿名函數運算式](expressions.md#anonymous-function-expressions)）是該*lambda_expression*的*anonymous_function_body* 。
*  在*anonymous_method_expression*中宣告的參數範圍（[匿名函數運算式](expressions.md#anonymous-function-expressions)）是該*anonymous_method_expression*的*區塊*。
*  在*labeled_statement*中宣告的標籤範圍（加上[標籤的語句](statements.md#labeled-statements)）是宣告發生所在的*區塊*。
*  在*local_variable_declaration*中宣告的本機變數範圍（[區域變數](statements.md#local-variable-declarations)宣告）是發生宣告的區塊。
*  在 @no__t 1 語句（[switch 語句](statements.md#the-switch-statement)）的*switch_block*中宣告的區域變數範圍是*switch_block*。
*  在 @no__t 1 語句（[for 語句](statements.md#the-for-statement)）的*for_initializer*中宣告的區域變數範圍是*for_initializer*、 *for_condition*、 *for_iterator*，以及的包含*語句*。`for` 語句。
*  在*local_constant_declaration*中宣告的本機常數範圍（[區域常數](statements.md#local-constant-declarations)宣告）是宣告發生所在的區塊。 這是編譯時期錯誤，可在其*constant_declarator*之前的文字位置參考本機常數。
*  宣告為*foreach_statement*、 *using_statement*、 *lock_statement*或*q*之一部分之變數的範圍是由指定結構的展開所決定。

在命名空間、類別、結構或列舉成員的範圍內，您可以在成員宣告之前，參考文字位置中的成員。 例如：
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
在這裡，它是有效的，`F` 會在宣告之前參考 `i`。

在本機變數的範圍內，會發生編譯時期錯誤，以在本機變數*local_variable_declarator*之前的文字位置參考本機變數。 例如：
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

在上述的 `F` 方法中，第一次指派至 `i` 特別不會參考外部範圍中所宣告的欄位。 相反地，它會參考區域變數，而且會導致編譯時期錯誤，因為它是以一詞的方式在變數的宣告前面。 在 `G` 方法中，在 `j` 宣告的初始化運算式中使用 `j` 是有效的，因為使用不在*local_variable_declarator*之前。 在 `H` 方法中，後續的*local_variable_declarator*會正確地參考在相同*local_variable_declaration*內先前*local_variable_declarator*中所宣告的本機變數。

本機變數的範圍規則是設計來保證運算式內容中所使用之名稱的意義在區塊內一律是相同的。 如果區域變數的範圍只會從其宣告延伸到區塊結尾，則在上述範例中，第一個指派會指派給執行個體變數，而第二個指派會指派給本機變數，可能會導致如果稍後要重新排列區塊的語句，就會發生編譯時期錯誤。

區塊內名稱的意義可能會根據使用名稱的內容而有所不同。 在範例中
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
運算式內容中會使用 `A` 的名稱來參考本機變數 `A`，並在類型內容中參考類別 `A`。

### <a name="name-hiding"></a>隱藏名稱

實體的範圍通常會包含比實體的宣告空間更多的程式文字。 特別是，實體的範圍可能包括引入新宣告空間的宣告，其中包含相同名稱的實體。 這類宣告會使原始實體變成***隱藏狀態***。 相反地，實體會在未隱藏時被視為***可見***。

範圍重迭時，會發生名稱隱藏，而範圍則是透過繼承來重迭。 下列各節將說明這兩種隱藏類型的特性。

#### <a name="hiding-through-nesting"></a>透過嵌套隱藏

藉由嵌套命名空間或命名空間內的型別，可能會因為在類別或結構內嵌套型別，以及做為參數和區域變數宣告的結果，而導致名稱隱藏。

在範例中
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
在 `F` 方法中，`i` 的執行個體變數會由本機變數 `i` 隱藏，但是在 `G` 方法中，`i` 仍然是指執行個體變數。

當內部範圍中的名稱隱藏外部範圍中的名稱時，它會隱藏該名稱的所有多載出現次數。 在範例中
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
呼叫 `F(1)` 會叫用 `Inner` 中所宣告的 `F`，因為內部宣告會隱藏所有 `F` 的外部出現專案。 基於相同的原因，呼叫 `F("Hello")` 會導致編譯時期錯誤。

#### <a name="hiding-through-inheritance"></a>透過繼承隱藏

當類別或結構重新宣告繼承自基類的名稱時，會透過繼承來隱藏名稱。 這種類型的名稱隱藏會採用下列其中一種形式：

*  在類別或結構中引進的常數、欄位、屬性、事件或類型，會隱藏所有具有相同名稱的基類成員。
*  在類別或結構中引進的方法會隱藏所有具有相同名稱的非方法基類成員，以及具有相同簽章的所有基類方法（方法名稱和參數計數、修飾詞和類型）。
*  在類別或結構中引進的索引子會隱藏所有具有相同簽章（參數計數和類型）的基類索引子。

管理運算子宣告（[運算子](classes.md#operators)）的規則讓衍生類別無法宣告運算子，其簽章與基類中的運算子具有相同的簽章。 因此，運算子絕對不會隱藏另一個。

相反地，隱藏外部範圍中的名稱，從繼承的範圍隱藏可存取的名稱會導致回報警告。 在範例中
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
`Derived` 中 `F` 的宣告會導致回報警告。 隱藏繼承的名稱特別不是錯誤，因為這會排除基類的個別進化。 例如，上述情況可能會出現，因為較新版本的 `Base` 引進了不存在於舊版類別中的 @no__t 1 方法。 如果上述情況是錯誤，則對個別版本化類別庫中的基類所做的任何變更，可能會導致衍生類別變成無效。

藉由使用 `new` 修飾詞，可以消除隱藏繼承名稱所造成的警告：
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

@No__t-0 修飾詞表示 `Derived` 中的 `F` 是 "new"，而它實際上是用來隱藏繼承的成員。

新成員的宣告只會在新成員的範圍內隱藏繼承的成員。

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

在上述範例中，`Derived` 中 `F` 的宣告會隱藏繼承自 `Base` 的 `F`，但因為 `Derived` 中新的 `F` 具有私用存取權，所以其範圍不會延伸至 `MoreDerived`。 因此，`MoreDerived.G` 中的呼叫 `F()` 是有效的，而且將會叫用 `Base.F`。

## <a name="namespace-and-type-names"></a>命名空間和類型名稱

程式中的數個內容需要指定*namespace_name*或*type_name。* C#

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

*Namespace_name*是參考命名空間的*namespace_or_type_name* 。 依照下面所述的解決方式， *namespace_name*的*namespace_or_type_name*必須參考命名空間，否則會發生編譯時期錯誤。 *Namespace_name*中不能有類型引數（[類型引數](types.md#type-arguments)）（只有類型可以有類型引數）。

*Type_name*是參考類型的*namespace_or_type_name* 。 如下所述的解決方法， *type_name*的*namespace_or_type_name*必須參考型別，否則會發生編譯時期錯誤。

如果*namespace_or_type_name*是限定別名成員，則其意義會如[命名空間別名限定詞](namespaces.md#namespace-alias-qualifiers)中所述。 否則， *namespace_or_type_name*會有四種形式的其中一種：

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

其中 `I` 是單一識別碼，`N` 是*namespace_or_type_name* ，而 `<A1, ..., Ak>` 是選擇性的*type_argument_list*。 未指定*type_argument_list*時，請考慮 `k` 為零。

*Namespace_or_type_name*的意義是依照下列方式決定：

*   如果*namespace_or_type_name*的格式為 `I`，或形式 `I<A1, ..., Ak>`：
    * 如果 `K` 為零，且*namespace_or_type_name*出現在泛型方法宣告（[方法](classes.md#methods)）中，而且該宣告包含名稱為 @ no__t-4 的型別參數（[型別參數](classes.md#type-parameters)），則*namespace_or_type_名稱*參考該類型參數。
    * 否則，如果*namespace_or_type_name*出現在類型宣告中，則針對每個實例類型 @ no__t-1 （[實例類型](classes.md#the-instance-type)），從該類型宣告的實例類型開始，然後繼續每個的實例類型。封入類別或結構宣告（如果有的話）：
        * 如果 `K` 為零，且 `T` 的宣告包含名稱為 @ no__t-2 的類型參數，則*namespace_or_type_name*會參考該類型參數。
        * 否則，如果*namespace_or_type_name*出現在類型宣告的主體內，而且 `T` 或其任何基底類型包含具有 name @ no__t-2 和 `K` @ no__t-4type 參數的嵌套可存取型別，則*namespace_or_type_name*是指以指定的型別引數所構成的型別。 如果有多個這種類型，則會選取在更多衍生類型內宣告的類型。 請注意，在決定的意義時，不會忽略類型成員（常數、欄位、方法、屬性、索引子、運算子、實例的函式、析構函式和靜態的函式）和類型成員的類型。*namespace_or_type_name*。
    * 如果先前的步驟不成功，則針對每個命名空間 @ no__t-0，從*namespace_or_type_name*發生所在的命名空間開始，繼續執行每個封入命名空間（如果有的話），並以全域命名空間結束，如下所示系統會評估步驟，直到實體找到為止：
        * 如果 `K` 為零，且 `I` 是 @ no__t-2 中的命名空間名稱，則：
            * 如果發生*namespace_or_type_name*的位置是以 `N` 的命名空間宣告括住，且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，而該名稱與 @ no 關聯具有命名空間或類型的 __t-4，則*namespace_or_type_name*是不明確的，而且會發生編譯時期錯誤。
            * 否則， *namespace_or_type_name*會參考 `N` 中名為 `I` 的命名空間。
        * 否則，如果 `N` 包含名稱為 @ no__t-1 的可存取類型和 `K` @ no__t-3type 參數，則：
            * 如果 `K` 為零，且發生*namespace_or_type_name*的位置是以 `N` 的命名空間宣告括住，且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，將名稱 @ no__t-5 關聯至命名空間或類型，然後*namespace_or_type_name*會不明確，且會發生編譯時期錯誤。
            * 否則， *namespace_or_type_name*會參考以指定的型別引數所構成的型別。
        * 否則，如果發生*namespace_or_type_name*的位置是由 `N` 的命名空間宣告所括住：
            * 如果 `K` 為零，且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，其會將名稱 @ no__t-3 與匯入的命名空間或類型產生關聯，則*namespace_or_type_name*會參考該命名空間或類型。
            * 否則，如果命名空間宣告的*using_namespace_directive*s 和*using_alias_directive*所匯入的命名空間和類型宣告，只包含一個名稱為 @ no__t-2 且 `K` @ no__t-4type 的可存取類型參數，然後*namespace_or_type_name*會參考以給定類型引數所構成的類型。
            * 否則，如果由命名空間宣告的*using_namespace_directive*s 和*using_alias_directive*所匯入的命名空間和類型宣告包含一個以上名稱為 @ no__t-2 且 `K` @ no__t-4type 的可存取類型參數，則*namespace_or_type_name*不明確，且會發生錯誤。
    * 否則， *namespace_or_type_name*會是未定義的，而且會發生編譯時期錯誤。
*  否則， *namespace_or_type_name*的格式為 `N.I`，或形式 `N.I<A1, ..., Ak>`。 `N` 會第一次解析為*namespace_or_type_name*。 如果 `N` 的解決方式不成功，就會發生編譯時期錯誤。 否則，`N.I` 或 `N.I<A1, ..., Ak>` 會解析如下：
    * 如果 `K` 為零，且 `N` 參考命名空間，而 `N` 包含名為 `I` 的嵌套命名空間，則*namespace_or_type_name*會參考該 nested 命名空間。
    * 否則，如果 `N` 指的是命名空間，而 `N` 包含具有 name @ no__t-2 和 `K` @ no__t-4type 參數的可存取型別，則*namespace_or_type_name*會參考以給定型別引數所結構化的型別。
    * 否則，如果 `N` 指的是（可能是已建立）的類別或結構類型，而 `N` 或其任何基類包含具有 name @ no__t-2 和 `K` @ no__t-4type 參數的嵌套可存取型別，則*namespace_or_type_name*會參考該型別是以指定的型別引數所構成。 如果有多個這種類型，則會選取在更多衍生類型內宣告的類型。 請注意，如果在解析 `N` 的基類規格中判斷 `N.I` 的意義，則 `N` 的直接基類會視為物件（[基類](classes.md#base-classes)）。
    * 否則，`N.I` 是不正確*namespace_or_type_name*，而且發生編譯時期錯誤。

只有在時，才允許*namespace_or_type_name*參考靜態類別（[靜態類別](classes.md#static-classes)）

*  *Namespace_or_type_name*是 `T.I` 格式的*namespace_or_type_name*中的 `T`，或
*  *Namespace_or_type_name*是表單`T` *typeof_expression* 的（[引數清單](expressions.md#argument-lists)1）中`typeof(T)`的。

### <a name="fully-qualified-names"></a>完整名稱

每個命名空間和類型都有***完整名稱***，可唯一識別命名空間或在所有其他專案之間的類型。 命名空間或類型的完整名稱 `N` 的決定方式如下：

*  如果 `N` 是全域命名空間的成員，其完整名稱會是 `N`。
*  否則，其完整名稱會 `S.N`，其中 `S` 是宣告 `N` 之命名空間或類型的完整名稱。

換句話說，`N` 的完整名稱是從全域命名空間開始，會導致 `N` 之識別碼的完整階層式路徑。 因為命名空間或類型的每個成員都必須有唯一的名稱，所以命名空間或類型的完整名稱一定是唯一的。

下列範例顯示數個命名空間和類型宣告，以及其相關聯的完整名稱。
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a>自動記憶體管理

C#採用自動記憶體管理，讓開發人員無法手動設定和釋放物件所佔用的記憶體。 自動記憶體管理原則是由***垃圾收集***行程所執行。 物件的記憶體管理生命週期如下所示：

1. 建立物件時，系統會為其配置記憶體、執行此函式，並將物件視為即時。
2. 如果任何可能的執行接續都無法存取物件（或其中的任何部分），則會將物件視為不再使用，而且它會變成符合終結的資格。 C#編譯器和垃圾收集行程可能會選擇分析程式碼，以判斷未來可能會使用的物件參考。 比方說，如果範圍內的區域變數是物件的唯一現有參考，但在程式的目前執行點執行任何可能的接續時，該區域變數絕對不會被參照，垃圾收集行程可能會（但不是必要）將物件視為不再使用。
3. 一旦物件符合終結條件的資格，在某些情況下，物件的析構函式（[析構](classes.md#destructors)函數）（如果有的話）會在未指定的時間執行。 在正常情況下，物件的析構函式只會執行一次，但執行特定的 Api 可能會允許覆寫此行為。
4. 一旦執行物件的析構函式之後，如果該物件或其中的任何部分無法透過任何可能的接續來存取（包括執行緒的執行），則會將物件視為無法存取，而且物件會變成符合集合的資格。
5. 最後，在物件變成可進行回收的一段時間後，垃圾收集行程就會釋出與該物件相關聯的記憶體。

垃圾收集行程會維護物件使用方式的相關資訊，並使用這項資訊來進行記憶體管理決策，例如在記憶體中尋找新建立的物件、何時重新放置物件，以及何時不再使用或無法存取物件。

就像其他假設垃圾收集行程存在的語言一樣， C#是設計成讓垃圾收集行程能夠執行各式各樣的記憶體管理原則。 例如， C#不需要執行析構函數，或在物件符合資格時立即予以收集，或是以任何特定的順序或在任何特定的執行緒上執行該析構函數。

您可以透過類別上的靜態方法，將垃圾收集行程的行為控制到某種程度，`System.GC`。 這個類別可以用來要求要進行的集合、要執行的析構函數（或不執行）等等。

由於垃圾收集行程允許寬緯度來決定收集物件和執行析構函數的時機，因此符合規範的執行可能會產生與下列程式碼所示不同的輸出。 程式
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
建立類別的實例 `A` 和類別的實例 `B`。 當指派 `null` 的值給 `b` 的變數時，這些物件就會成為垃圾收集的資格，因為在這段時間之後，任何使用者撰寫的程式碼都無法存取它們。 輸出可以是

```console
Destruct instance of A
Destruct instance of B
```
或
```console
Destruct instance of B
Destruct instance of A
```
因為此語言對物件進行垃圾收集的順序沒有限制。

在微妙的情況下，「符合終結資格」和「符合集合資格」的差異可能很重要。 例如，套用至物件的
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

在上述程式中，如果垃圾收集行程選擇在 `B` 的析構函式之前執行 `A` 的析構函數，則此程式的輸出可能會是：
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

請注意，雖然 `A` 的實例不在使用中，且已執行 `A` 的析構函式，但仍有可能從另一個析構函式呼叫 `A` （在此案例中為 `F`）的方法。 此外，請注意，執行析構函式可能會再次導致物件可從主執行緒序中使用。 在此情況下，執行 @no__t 0 的「析構函式」會導致先前未使用的 `A` 實例變成可從即時參考 `Test.RefA` 存取。 在呼叫 `WaitForPendingFinalizers` 之後，`B` 的實例就適合進行集合，但 `A` 的實例則不是，因為參考 `Test.RefA`。

為了避免混淆和非預期的行為，讓析構函數只能對儲存在其物件本身欄位中的資料執行清除，而不是在參考的物件或靜態欄位上執行任何動作，通常是個不錯的主意。

使用析構函數的替代方法是讓類別實作為 @no__t 0 介面。 這可讓物件的用戶端判斷何時釋放物件的資源，通常是以 @no__t 0 的語句（[using 語句](statements.md#the-using-statement)）中的資源存取物件。

## <a name="execution-order"></a>執行順序

執行C#程式會繼續進行，讓每個執行中線程的副作用都會保留在關鍵執行點上。 ***副作用***會定義為 volatile 欄位的讀取或寫入、對非 volatile 變數的寫入、對外部資源的寫入，以及擲回例外狀況。 這些副作用的順序必須保留的關鍵執行點是變動性欄位（[volatile 欄位](classes.md#volatile-fields)）、@no__t 1 語句（[lock 語句](statements.md#the-lock-statement)）和執行緒建立和終止的參考。 執行環境可以自由變更C#程式的執行順序，但受限於下列條件約束：

*  資料相關性會在執行的執行緒中保留。 也就是說，每個變數的值都會計算，就像執行緒中的所有語句都是以原始程式循序執行一樣。
*  系統會保留初始化順序規則（[欄位初始化](classes.md#field-initialization)和[變數初始化運算式](classes.md#variable-initializers)）。
*  副作用的順序會根據變動性讀取和寫入（[volatile 欄位](classes.md#volatile-fields)）來保留。 此外，如果執行環境可以推算運算式的值未使用，而且不會產生任何所需的副作用（包括任何因呼叫方法或存取 volatile 欄位所造成的結果），則不需要評估運算式的一部分。 當非同步事件中斷程式執行時（例如由另一個執行緒擲回的例外狀況），不保證會在原始程式順序中看到可觀察的副作用。
