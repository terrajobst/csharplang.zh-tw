---
ms.openlocfilehash: 1c3d05674f8f7b69e70e0d9e06021537fc45f7ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488493"
---
# <a name="basic-concepts"></a>基本概念

## <a name="application-startup"></a>應用程式啟動

組件具有***進入點***稱為***應用程式***。 應用程式時執行，新***應用程式定義域***建立。 數個不同的具現化的應用程式可能存在同一部電腦上，在此同時，而且各有自己的應用程式定義域。

應用程式定義域做為應用程式狀態的容器，以啟用應用程式隔離。 應用程式定義域做為容器和應用程式和它所使用的類別庫中定義之類型的界限。 一個應用程式定義域所載入的型別是不同的載入另一個應用程式定義域，相同的型別和物件的執行個體無法直接共用應用程式定義域之間。 比方說，每個應用程式定義域都有自己一份靜態變數，針對這些類型，和類型的靜態建構函式執行一次，每個應用程式網域。 實作可自行提供實作特定原則或機制的建立和解構的應用程式定義域。

***應用程式啟動***發生於執行環境會呼叫指定的方法，指應用程式的進入點。 此進入點方法的名稱永遠`Main`，且可以有一個下列簽章：

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

如所示，進入點可以選擇性地傳回`int`值。 這會傳回值會在應用程式終止 ([應用程式終止](basic-concepts.md#application-termination))。

進入點可能會選擇一個型式參數。 參數可能會有任何名稱，但必須是參數的型別`string[]`。 如果有型式參數，執行環境建立並傳遞`string[]`引數包含命令列引數所指定的應用程式啟動時。 `string[]`引數絕不會是 null，但它可能會有長度為零，如果未不指定任何命令列引數。

由於 C# 支援多載的方法，類別或結構可能包含多個定義的一些方法，都各自具有不同的簽章。 不過，在單一程式中，任何類別或結構可以包含多個方法呼叫`Main`定義已限定它用做為應用程式進入點。 其他多載的版本`Main`可以，不過，提供了一個以上的參數，或其唯一參數是型別以外`string[]`。

應用程式可以組成多個類別或結構。 您有一個以上的這些類別或結構以包含呼叫的方法`Main`定義已限定它用做為應用程式進入點。 在此情況下，必須使用外部機制 （例如命令列編譯器選項） 來選取其中一種`Main`做為進入點方法。

在 C# 中，每個方法都必須定義為類別或結構的成員。 一般情況下，宣告存取範圍 ([宣告存取範圍](basic-concepts.md#declared-accessibility)) 的方法取決於存取修飾詞 ([存取修飾詞](classes.md#access-modifiers)) 中指定其宣告，但同樣的宣告型別的存取範圍取決於其宣告中指定的存取修飾詞。 為了指定型別的指定方法要能夠呼叫，型別和成員必須是可存取。 不過，應用程式進入點是特殊案例。 具體來說，執行環境可以存取應用程式的進入點，無論其已宣告存取範圍，也不論其封入型別宣告的宣告存取範圍。

應用程式進入點方法可能不在泛型類別宣告中。

在其他方面，進入點方法的行為類似的不是進入點。

## <a name="application-termination"></a>應用程式終止

***應用程式終止***將控制權傳回給執行環境。

如果傳回類型的應用程式***進入點***方式`int`，傳回的值做為應用程式的***終止狀態碼***。 此程式碼的目的是允許的成功或失敗的執行環境的通訊。

進入點方法的傳回型別是否`void`，到達右大括號 (`}`)，這會終止方法，或執行`return`陳述式沒有運算式，會導致終止狀態碼`0`。

應用程式的終止前的所有其尚未經過記憶體回收的物件解構函式呼叫時，除非已隱藏這類清除 (程式庫方法呼叫`GC.SuppressFinalize`，例如)。

## <a name="declarations"></a>宣告

在 C# 程式中的宣告會定義程式的組成項目。 C# 程式組織方式使用命名空間 ([命名空間](namespaces.md))，其中可以包含類型宣告和巢狀命名空間宣告。 型別宣告 ([型別宣告](namespaces.md#type-declarations)) 用來定義類別 ([類別](classes.md))，結構 ([結構](structs.md))，介面 ([介面](interfaces.md))，列舉 ([列舉](enums.md))，和委派 ([委派](delegates.md))。 型別宣告中允許的成員類型取決於型別宣告的形式。 比方說，類別的宣告可以包含常數的宣告 ([常數](classes.md#constants))，欄位 ([欄位](classes.md#fields))，方法 ([方法](classes.md#methods))，屬性 ([屬性](classes.md#properties))，事件 ([事件](classes.md#events))，索引子 ([的索引子](classes.md#indexers))，運算子 ([運算子](classes.md#operators))，執行個體建構函式 ([執行個體建構函式](classes.md#instance-constructors))，靜態建構函式 ([靜態建構函式](classes.md#static-constructors))，解構函式 ([解構函式](classes.md#destructors))，以及巢狀類型 ([巢狀型別](classes.md#nested-types)).

宣告會定義中的名稱***宣告空間***所屬的宣告。 除了多載成員 ([簽章和多載](basic-concepts.md#signatures-and-overloading))，它是編譯時期錯誤有兩個或多個宣告引入的宣告空間中同名的成員。 您不可能包含不同類型的成員具有相同名稱的宣告空間。 例如，宣告空間可以永遠不會包含欄位和方法，以相同的名稱。

有許多不同類型的宣告空間，如下列所述。

*  內的程式，所有的原始程式檔*namespace_member_declaration*秒，沒有封入*namespace_declaration*屬於稱為單一合併的宣告空間***全域宣告空間***。
*  中的程式，所有的原始程式檔*namespace_member_declaration*內*namespace_declaration*具有相同的完整命名空間名稱都是單一的結合宣告的成員空間。
*  每個類別、 結構或介面宣告會建立新的宣告空間。 名稱會導入此宣告空間*class_member_declaration*s *struct_member_declaration*s *interface_member_declaration*s，或*type_parameter*s。 除了多載的執行個體建構函式宣告和靜態建構函式宣告、 類別或結構不能包含具有相同名稱的類別或結構宣告的成員。 類別、 結構或介面允許多載的方法和索引子的宣告。 此外，類別或結構允許多載的執行個體建構函式和運算子的宣告。 比方說，類別、 結構或介面可能包含多個方法宣告具有相同的名稱，提供這些方法宣告不同，其簽章中 ([簽章和多載](basic-concepts.md#signatures-and-overloading))。 請注意，類別的宣告空間並不會提供基底類別的基底介面並不會提供介面的宣告空間。 因此，衍生的類別或介面允許宣告具有相同名稱的成員，做為繼承的成員。 這類成員即為***隱藏***繼承的成員。
*  每個委派宣告會建立新的宣告空間。 名稱會導入此型式參數的宣告空間 (*fixed_parameter*s 並*parameter_array*s) 和*type_parameter*s。
*  每個列舉型別宣告會建立新的宣告空間。 名稱會導入此宣告空間*enum_member_declarations*。
*  每個方法宣告、 宣告索引子、 運算子宣告、 執行個體建構函式宣告和匿名函式會建立新的宣告空間，稱為***區域變數宣告空間***。 名稱會導入此型式參數的宣告空間 (*fixed_parameter*s 並*parameter_array*s) 和*type_parameter*s。 主體的函式成員 」 或 「 匿名函式，如果有的話，視為巢狀於本機變數的宣告空間。 它是本機變數的宣告空間和巢狀的本機變數宣告空間，以包含具有相同名稱的項目時發生。 因此，在巢狀的宣告空間內不可以在封入的宣告空間中宣告本機變數或常數，其名稱相同的本機變數或常數。 可以包含具有相同名稱的項目，只要空間不包含其他兩個宣告空間。
*  每個*區塊*或*switch_block* ，以及*如*， *foreach*並*使用*陳述式，會建立區域變數宣告區域變數和區域常數的空間。 名稱會導入此宣告空間*local_variable_declaration*s 並*local_constant_declaration*s。 請注意，這些函式宣告為參數的本機變數的宣告空間內巢狀或函式成員或匿名函式主體內，就會發生的區塊。 因此，它是錯誤的本機變數與相同名稱的參數，方法。
*  每個*區塊*或是*switch_block*建立個別的宣告空間的標籤。 名稱會導入此宣告空間*labeled_statement*和名稱會透過參考*goto_statement*s。 ***標籤宣告空間***區塊包含任何巢狀的區塊。 因此，巢狀區塊內不可以宣告具有相同名稱做為標籤，以在封閉區塊中的標籤。

名稱宣告的文字順序通常是沒有意義。 特別是，文字順序並不重要的宣告和使用命名空間、 常數、 方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式、 靜態的建構函式和類型。 依宣告順序是重要功能如下：

*  欄位宣告的區域變數宣告的宣告順序決定其初始設定式 （如果有的話） 執行的順序。
*  必須定義區域變數，再使用它們 ([範圍](basic-concepts.md#scopes))。
*  列舉的成員宣告的宣告順序 ([列舉成員](enums.md#enum-members)) 時，重要*constant_expression*會省略值。

命名空間的宣告空間是 「 開啟已結束 」，而兩個命名空間宣告具有相同的完整名稱提供給相同的宣告空間。 例如
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

上面的兩個命名空間宣告提供給相同的宣告空間，在此情況下宣告兩個類別的完整格式名稱具有`Megacorp.Data.Customer`和`Megacorp.Data.Order`。 這兩種宣告參與相同的宣告空間，因為它會造成編譯時期錯誤如果每一個包含具有相同名稱的類別的宣告。

如上所述，在區塊的宣告空間會包含任何巢狀的區塊。 因此，在下列範例中，`F`並`G`方法會產生編譯時期錯誤，因為名稱`i`外部區塊中宣告，因此不能重新宣告中的內部區塊。 不過，`H`並`I`方法都有效，因為這兩個`i`的另一個非巢狀區塊中宣告。

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

命名空間和型別具有***成員***。 實體的成員是透過限定名稱，後面接著參考的實體，開始使用正式推出 」`.`"語彙基元，後面接著的成員名稱。

類型宣告中是宣告類型的成員或***繼承***型別的基底類別。 當型別繼承自基底類別的基底類別的執行個體建構函式、 解構函式和靜態建構函式除外的所有成員會變成衍生型別的成員。 基底類別成員的宣告存取範圍不會控制是否要繼承的成員 — 繼承會延伸到任何不是執行個體建構函式、 靜態的建構函式或解構函式的成員。 不過，繼承的成員可能無法存取在衍生的型別，因為其已宣告存取範圍 ([宣告存取範圍](basic-concepts.md#declared-accessibility)) 或因為它隱藏的宣告型別本身 ([透過隱藏繼承](basic-concepts.md#hiding-through-inheritance))。

### <a name="namespace-members"></a>命名空間成員

命名空間和型別有沒有封入命名空間都隸屬***全域命名空間***。 這直接對應到全域宣告空間中宣告的名稱。

命名空間和命名空間內宣告的型別是該命名空間的成員。 這直接對應至命名空間的宣告空間中宣告的名稱。

命名空間沒有存取限制。 它不可以宣告 private、 protected 或 internal 的命名空間，而且命名空間名稱永遠是可公開存取。

### <a name="struct-members"></a>結構成員

結構成員會在結構中宣告的成員和繼承自結構的直接基底類別成員`System.ValueType`間接基底類別和`object`。

簡單類型的成員直接對應到此結構的類型別名的簡單類型的成員：

*  成員`sbyte`的成員包括`System.SByte`結構。
*  成員`byte`的成員包括`System.Byte`結構。
*  成員`short`的成員包括`System.Int16`結構。
*  成員`ushort`的成員包括`System.UInt16`結構。
*  成員`int`的成員包括`System.Int32`結構。
*  成員`uint`的成員包括`System.UInt32`結構。
*  成員`long`的成員包括`System.Int64`結構。
*  成員`ulong`的成員包括`System.UInt64`結構。
*  成員`char`的成員包括`System.Char`結構。
*  成員`float`的成員包括`System.Single`結構。
*  成員`double`的成員包括`System.Double`結構。
*  成員`decimal`的成員包括`System.Decimal`結構。
*  成員`bool`的成員包括`System.Boolean`結構。

### <a name="enumeration-members"></a>列舉型別成員

列舉的成員會宣告為列舉中的常數和列舉型別的直接基底類別繼承的成員`System.Enum`和間接基底類別`System.ValueType`和`object`。

### <a name="class-members"></a>類別成員

類別的成員會在類別中宣告的成員和繼承自基底類別成員 (除了類別`object`具有沒有基底類別)。 繼承自基底類別的成員包括常數、 欄位、 方法、 屬性、 事件、 索引子、 運算子和類型的基底類別，但不是執行個體建構函式、 解構函式和基底類別的靜態建構函式。 基底類別成員會繼承其存取範圍無關。

在類別宣告可能包含常數、 欄位、 方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式、 靜態建構函式和類型的宣告。

成員`object`和`string`直接對應到類別類型的成員在別名：

*  成員`object`的成員包括`System.Object`類別。
*  成員`string`的成員包括`System.String`類別。

### <a name="interface-members"></a>介面成員

介面的成員是在介面和介面的所有基底介面中宣告的成員。 在類別成員`object`則不會，嚴格地說，任何介面的成員 ([介面成員](interfaces.md#interface-members))。 不過，在類別成員`object`可透過任何介面類型中進行成員查詢 ([成員查閱](expressions.md#member-lookup))。

### <a name="array-members"></a>陣列成員

陣列的成員會繼承自類別的成員`System.Array`。

### <a name="delegate-members"></a>委派成員

委派的成員會繼承自類別的成員`System.Delegate`。

## <a name="member-access"></a>成員存取

成員宣告允許成員存取控制。 成員存取範圍藉由宣告存取範圍 ([宣告存取範圍](basic-concepts.md#declared-accessibility)) 成員的結合可存取性，立即包含類型中，如果有的話。

當允許存取特定的成員時，該成員即為***存取***。 相反地，當不被允許存取特定的成員，該成員要***無法存取***。 存取範圍定義域中包含文字的位置存取發生時，允許存取成員 ([存取範圍定義域](basic-concepts.md#accessibility-domains)) 的成員。

### <a name="declared-accessibility"></a>已宣告存取範圍

***宣告存取範圍***的成員可以是下列其中之一：

*  公用、 選取包含`public`成員宣告中的修飾詞。 直覺式的意義`public`是 「 不受限制的存取 」。
*  受保護，會選取包括`protected`成員宣告中的修飾詞。 直覺式的意義`protected`」 存取僅限於包含類別或類型衍生自包含類別 」。
*  內部，會選取 包括`internal`成員宣告中的修飾詞。 直覺式的意義`internal`是 「 限於此程式的存取 」。
*  受保護的內部 （亦即 protected 或 internal），會選取包括`protected`和`internal`成員宣告中的修飾詞。 直覺式的意義`protected internal`是 「 存取僅限於此程式或衍生自包含類別的型別 」。
*  選取包含私用`private`成員宣告中的修飾詞。 直覺式的意義`private`是 「 存取限於包含型別 」。

根據宣告的成員所發生的內容，允許特定類型的宣告存取範圍。 此外，當成員宣告不包含任何存取修飾詞，宣告進行的內容會決定預設的宣告存取範圍。

*  命名空間以隱含方式具有`public`宣告存取範圍。 命名空間宣告中不能使用任何存取修飾詞。
*  在編譯單位或命名空間中宣告的型別可以有`public`或是`internal`宣告存取範圍和預設值是`internal`宣告存取範圍。
*  類別成員可以有五種類型的宣告存取範圍，預設為`private`宣告存取範圍。 (請注意，型別宣告為類別的成員可以具有任何五種類型的宣告存取範圍，而型別宣告為命名空間的成員可以只有`public`或`internal`宣告存取範圍。)
*  結構的成員可以有`public`， `internal`，或`private`宣告存取範圍和預設值是`private`宣告存取範圍，因為會隱含地密封結構。 結構 （也就不會繼承該結構） 中導入的結構成員不能有`protected`或`protected internal`宣告存取範圍。 (請注意，型別宣告為結構的成員可以有`public`， `internal`，或`private`宣告存取範圍，而型別宣告為命名空間的成員可以只有`public`或`internal`宣告存取範圍。)
*  介面成員以隱含方式具有`public`宣告存取範圍。 介面成員宣告中不能使用任何存取修飾詞。
*  列舉型別成員以隱含方式具有`public`宣告存取範圍。 列舉型別成員宣告中不能使用任何存取修飾詞。

### <a name="accessibility-domains"></a>存取範圍定義域

***存取範圍定義域***的成員包含的程式文字中允許的成員存取 （可能是脫離） 的各節。 為了定義成員的存取範圍定義域，成員要***最上層***如果它未宣告的型別中，而且成員要***巢狀***如果另一個類型內宣告。 此外，***程式文字***程式的定義為所有程式的程式，所有的原始程式檔中所包含的文字，並且程式文字類型的定義為所有的程式中包含文字*type_declaration*s 該型別 （包括，可能巢狀類型的類型）。

預先定義的類型的存取範圍定義域 (例如`object`， `int`，或`double`) 不受限制。

最上層的存取範圍定義域未繫結的型別`T`([繫結和解除繫結型別](types.md#bound-and-unbound-types)) 在程式中宣告`P`定義如下：

*  如果宣告存取範圍`T`是`public`的存取範圍定義域`T`是程式文字`P`和 參考的任何程式`P`。
*  如果 `T` 的宣告存取範圍是 `internal`，`T` 的存取範圍領域就是 `P` 的程式文字。

這些定義它如下所示，最上層的未繫結類型的存取範圍定義域一律至少是在宣告中，輸入的的程式的程式文字。

建構類型的存取範圍定義域`T<A1, ..., An>`交集的未繫結的泛型類型的存取範圍定義域`T`和類型引數的存取範圍定義域`A1, ..., An`。

巢狀成員的存取範圍定義域`M`型別宣告`T`程式內`P`定義，如下所示 (注意的是，`M`本身可能就是類型):

*  如果 `M` 的宣告存取範圍是 `public`，`M` 的存取範圍領域就是 `T` 的存取範圍領域。
*  如果宣告存取範圍`M`是`protected internal`讓`D`是等位的程式文字`P`以及任何類型的程式文字衍生自`T`，外部宣告的所在`P`。 存取範圍定義域`M`交集的存取範圍領域`T`使用`D`。
*  如果宣告存取範圍`M`是`protected`讓`D`是等位的程式文字`T`以及任何類型的程式文字衍生自`T`。 存取範圍定義域`M`交集的存取範圍領域`T`使用`D`。
*  如果 `M` 的宣告存取範圍是 `internal`，`M` 的存取範圍領域是 `T` 的存取範圍領域與 `P` 的程式文字的交集。
*  如果 `M` 的宣告存取範圍是 `private`，`M` 的存取範圍領域就是 `T` 的程式文字。

這些定義它如下所示的巢狀成員的存取範圍領域是一律至少在其中宣告成員型別的程式文字。 此外，它會遵循成員存取範圍領域是永遠不會宣告成員的型別存取範圍領域與更廣。

簡單來說，類型或成員時`M`是存取，會評估下列步驟以確保允許的存取：

*  首先，如果`M`在宣告的類型 （但不要編譯單位或命名空間），如果該類型不能存取，就會發生編譯時期錯誤。
*  然後，如果`M`是`public`，允許存取。
*  否則，如果`M`是`protected internal`，允許的存取，以在程式內發生`M`宣告，如果它就會發生在類別或衍生自類別所在`M`宣告並透過衍生類別型別 ([受保護執行個體存取成員](basic-concepts.md#protected-access-for-instance-members))。
*  否則，如果`M`是`protected`，所在類別內發生，允許的存取`M`宣告，如果它就會發生在類別或衍生自類別所在`M`宣告並透過衍生類別型別 ([受保護執行個體存取成員](basic-concepts.md#protected-access-for-instance-members))。
*  否則，如果`M`已`internal`，以在程式內發生，允許的存取`M`宣告。
*  否則，如果`M`已`private`，在其中的型別內發生，允許的存取`M`宣告。
*  否則，型別或成員無法存取，而且就會發生編譯時期錯誤。

在範例
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
類別和成員具有下列的存取範圍定義域：

*  存取範圍定義域`A`和`A.X`不受限制。
*  存取範圍定義域`A.Y`， `B`， `B.X`， `B.Y`， `B.C`， `B.C.X`，和`B.C.Y`是包含程式的程式文字。
*  存取範圍定義域`A.Z`是程式文字`A`。
*  存取範圍定義域`B.Z`並`B.D`是程式文字`B`，包括的程式文字`B.C`和`B.D`。
*  存取範圍定義域`B.C.Z`是程式文字`B.C`。
*  存取範圍定義域`B.D.X`並`B.D.Y`是程式文字`B`，包括的程式文字`B.C`和`B.D`。
*  存取範圍定義域`B.D.Z`是程式文字`B.D`。

如範例所示，成員存取範圍領域是類型的永遠不會超過包含。 比方說，即使所有`X`成員具有公用的宣告存取範圍以外的所有`A.X`有會受限於包含類型的存取範圍定義域。

中所述[成員](basic-concepts.md#members)，衍生型別會繼承基底類別，執行個體建構函式、 解構函式和靜態建構函式除外的所有成員。 這包括基底類別的平均私用成員。 不過，在私用成員的存取範圍定義域包含只能宣告成員類型的程式文字。 在範例
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
`B`類別會繼承私用成員`x`從`A`類別。 因為是私用成員，才可存取*class_body*的`A`。 因此，若要存取`b.x`成功地`A.F`方法，但在失敗`B.F`方法。

### <a name="protected-access-for-instance-members"></a>受保護執行個體成員的存取

當`protected`執行個體成員存取以外的程式文字的類別中宣告，及何時`protected internal`執行個體成員存取外部程式宣告它的程式文字、 存取權必須在進行中衍生自其宣告所在類別的類別宣告。 此外，所以需要透過該衍生的類別類型或從中建構類別類型的執行個體進行存取。 這項限制會防止存取的其他衍生的類別，受保護的成員，即使成員繼承自相同基底類別所衍生的類別。

可讓`B`是宣告受保護執行個體成員的基底類別`M`，並讓`D`是衍生自類別`B`。 內*class_body*的`D`，存取`M`可以採用下列格式之一：

*  不合格*type_name*或是*primary_expression*表單的`M`。
*  A *primary_expression*的表單`E.M`，提供的型別`E`是`T`或衍生自類別`T`，其中`T`是類別類型`D`，或類別類型從建構 `D`
*  A *primary_expression*表單的`base.M`。

除了這些形式的存取權限，在衍生的類別可以存取受保護執行個體建構函式中的基底類別*constructor_initializer* ([建構函式初始設定式](classes.md#constructor-initializers))。

在範例
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
內`A`，就能夠存取`x`透過兩個執行個體`A`並`B`，因為在任一情況下存取是透過執行個體進行`A`或衍生自類別`A`。 不過，在`B`，就不能夠存取`x`的執行個體透過`A`，因為`A`不是衍生自`B`。

在範例
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
三個指派`x`已允許，因為它們全都需要透過建構自泛型類型的類別類型的執行個體。

### <a name="accessibility-constraints"></a>存取範圍條件約束

在 C# 語言中的數個建構需要為類型***至少像那樣***成員或另一個型別。 型別`T`即為至少一個成員或類型像那樣`M`如果的存取範圍定義域`T`的存取範圍定義域的超集`M`。 亦即`T`是至少像那樣`M`如果`T`所在的所有內容中存取`M`存取。

協助工具條件約束如下：

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
*  執行個體建構函式的參數類型必須是至少一樣地存取本身的執行個體建構函式。

在範例
```csharp
class A {...}

public class B: A {...}
```
`B`類別會產生編譯時期錯誤，因為`A`不是至少像那樣`B`。

同樣地，在範例
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
`H`方法中的`B`導致編譯時期錯誤，因為傳回型別`A`不至少像方法那樣。

## <a name="signatures-and-overloading"></a>簽章和多載

方法、 執行個體建構函式、 索引子和運算子的特性在於他們***簽章***:

*  方法的簽章包含方法、 類型參數的數目和類型和類型 （值、 參考或輸出） 的每個型式參數，由左到右的順序的名稱。 對於這些用途，不是由其名稱，但類型引數清單中之方法的序數位置識別之方法的型式參數的型別，就會發生的任何型別參數。 方法的簽章特別不包含傳回的型別，`params`修飾詞，可供指定的最右邊的參數或選擇性型別參數條件約束。
*  執行個體建構函式的簽章包含型別和類型 （值、 參考或輸出） 的每個型式參數，由左到右的順序。 執行個體建構函式的簽章特別不包含`params`修飾詞，可供指定的最右邊的參數。
*  索引子的簽章所組成的每個型式參數，由左到右的順序型別。 索引子的簽章特別不包含此項目類型，也不會包含`params`修飾詞，可供指定的最右邊的參數。
*  運算子的簽章所組成的運算子和其型式參數，由左到右的順序的每個型別的名稱。 特別是運算子的簽章不包含的結果型別。

簽章是啟用機制***多載***類別、 結構和介面中的成員：

*  方法的多載，可讓類別、 結構或介面宣告具有相同名稱，並提供它們的簽章的多個方法中是唯一的類別、 結構或介面。
*  執行個體建構函式多載允許類別或結構宣告多個執行個體建構函式，前提是它們的簽章是該類別或結構內唯一的。
*  多載的索引子允許類別、 結構或介面宣告多個索引子，前提是它們的簽章中的類別、 結構或介面的唯一的。
*  多載的運算子允許類別或結構宣告具有相同名稱，並提供它們的簽章的多個運算子都是唯一的類別或結構內。

雖然`out`並`ref`參數修飾詞會視為簽章的一部分，在單一類型中宣告的成員不能單獨由不同簽章中`ref`和`out`。 如果兩個成員會宣告相同的型別會是相同的簽章中使用這兩種方法中的所有參數，就會發生編譯時期錯誤`out`修飾詞變更為`ref`修飾詞。 用於簽章相符的其他用途 （例如隱藏或覆寫），`ref`和`out`視為簽章的一部分，且彼此不相符。 (這項限制可讓C#程式來執行在 Common Language Infrastructure (CLI)，這不會提供方法來定義只會在不同的方法，輕易地轉譯`ref`並`out`。)

簽章類型基於`object`和`dynamic`都會視為相同。 單一類型中宣告的成員可以因此是相同的簽章單獨由`object`和`dynamic`。

下列範例會顯示一組多載的方法宣告，以及它們的簽章。
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

請注意任何`ref`並`out`參數修飾詞 ([方法參數](classes.md#method-parameters)) 簽章的一部分。 因此，`F(int)`和`F(ref int)`是唯一的簽章。 不過，`F(ref int)`並`F(out int)`不能在相同的介面中宣告，因為它們的簽章單獨由不同`ref`和`out`。 另外，請注意，傳回型別和`params`修飾詞不屬於簽章，因此無法單獨根據傳回型別或包含或排除的多載`params`修飾詞。 這樣一來，一種方法的宣告`F(int)`和`F(params string[])`上述導致編譯時期錯誤。

## <a name="scopes"></a>範圍

***範圍***名稱是在其中是可以參考的名稱，但是不限定的名稱所宣告的實體的程式文字的區域。 範圍可以***巢狀***，和內部範圍可能會重新宣告的名稱來自外部範圍的意義 (這不會不過，移除所加諸的限制[宣告](basic-concepts.md#declarations)巢狀區塊內不是可以宣告區域變數與封閉區塊中的區域變數名稱相同）。 來自外部範圍的名稱然後要***隱藏***區域中的程式文字涵蓋內部範圍中，而存取的外部名稱也僅能由限定名稱。

*  命名空間成員的範圍宣告*namespace_member_declaration* ([命名空間成員](namespaces.md#namespace-members)) 與沒有封入*namespace_declaration*是整個計劃文字。
*  命名空間成員的範圍宣告*namespace_member_declaration*內*namespace_declaration*其完整的名稱是`N`是*namespace_body*的每個*namespace_declaration*其完整的名稱是`N`開頭或`N`，後面接著句號。
*  名稱所定義的範圍*extern_alias_directive*擴充*using_directive*s *global_attributes*並*namespace_member_宣告*立即包含編譯單位或命名空間主體的 s。 *Extern_alias_directive*並沒有提供給基礎的宣告空間的任何新成員。 亦即*extern_alias_directive*不是可轉移的但相反地，會影響僅編譯單位或命名空間主體中發生。
*  名稱的範圍定義，或藉由匯入*using_directive* ([Using 指示詞](namespaces.md#using-directives)) 擴充*namespace_member_declaration*s 的*compilation_unit*或是*namespace_body*所在*using_directive* ，就會發生。 A *using_directive*得零個或多個命名空間、 類型或成員名稱可以在特定*compilation_unit*或是*namespace_body*，但不會提供給基礎的宣告空間的任何新成員。 亦即*using_directive*不是可轉移，但只會影響*compilation_unit*或是*namespace_body*它發生所在。
*  所宣告的型別參數的範圍*type_parameter_list*上*class_declaration* ([類別宣告](classes.md#class-declarations)) 是*class_base*， *type_parameter_constraints_clause*s，並*class_body*該*class_declaration*。
*  所宣告的型別參數的範圍*type_parameter_list*上*struct_declaration* ([結構宣告](structs.md#struct-declarations)) 是*struct_interfaces*， *type_parameter_constraints_clause*s，並*struct_body*該*struct_declaration*。
*  所宣告的型別參數的範圍*type_parameter_list*上*interface_declaration* ([介面宣告](interfaces.md#interface-declarations)) 是*interface_base*， *type_parameter_constraints_clause*s，並*interface_body*該*interface_declaration*。
*  所宣告的型別參數的範圍*type_parameter_list*上*delegate_declaration* ([委派宣告](delegates.md#delegate-declarations)) 是*return_type*， *formal_parameter_list*，以及*type_parameter_constraints_clause*的 s *delegate_declaration*。
*  所宣告的成員範圍*class_member_declaration* ([類別主體](classes.md#class-body)) 會*class_body*中宣告已發生。 此外，類別成員的範圍會延伸到*class_body*的衍生類別中的存取範圍領域包含 ([存取範圍定義域](basic-concepts.md#accessibility-domains)) 的成員。
*  所宣告的成員範圍*struct_member_declaration* ([結構成員](structs.md#struct-members)) 會*struct_body*中宣告已發生。
*  所宣告的成員範圍*enum_member_declaration* ([列舉成員](enums.md#enum-members)) 會*enum_body*中宣告已發生。
*  參數的範圍中宣告*method_declaration* ([方法](classes.md#methods)) 會*method_body*該*method_declaration*。
*  參數的範圍中宣告*indexer_declaration* ([的索引子](classes.md#indexers)) 會*accessor_declarations*的*indexer_declaration*.
*  參數的範圍中宣告*operator_declaration* ([運算子](classes.md#operators)) 會*區塊*該*operator_declaration*。
*  參數的範圍中宣告*constructor_declaration* ([執行個體建構函式](classes.md#instance-constructors)) 會*constructor_initializer*並*區塊*該*constructor_declaration*。
*  參數的範圍中宣告*lambda_expression* ([匿名函式運算式](expressions.md#anonymous-function-expressions)) 會*anonymous_function_body*的*lambda_運算式*
*  參數的範圍中宣告*anonymous_method_expression* ([匿名函式運算式](expressions.md#anonymous-function-expressions)) 會*區塊*的*anonymous_method_expression*。
*  標記的範圍中宣告*labeled_statement* ([標示為陳述式](statements.md#labeled-statements)) 會*區塊*中宣告已發生。
*  在宣告的本機變數的範圍*local_variable_declaration* ([區域變數宣告](statements.md#local-variable-declarations)) 區塊中宣告已發生。
*  範圍中宣告的區域變數*switch_block*的`switch`陳述式 ([switch 陳述式](statements.md#the-switch-statement)) 是*switch_block*。
*  範圍中宣告的區域變數*for_initializer*的`for`陳述式 ([陳述式](statements.md#the-for-statement)) 是*for_initializer*， *for_condition*，則*for_iterator*，並包含*陳述式*的`for`陳述式。
*  本機常數的範圍中宣告*local_constant_declaration* ([區域常數宣告](statements.md#local-constant-declarations)) 是在區塊中宣告已發生。 它是編譯時期錯誤之前的文字位置的區域常數是指其*constant_declarator*。
*  變數的範圍宣告的一部分*foreach_statement*， *using_statement*， *lock_statement*或是*query_expression>* 是取決於擴充的指定建構。

範圍內的命名空間、 類別、 結構或列舉的成員就可以參考之前的成員宣告的文字位置中的成員。 例如
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
在這裡，它是適用於`F`來參考`i`其宣告之前。

本機變數的範圍，它是參考之前的文字位置的本機變數的編譯時期錯誤*local_variable_declarator*的區域變數。 例如
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

在 `F`上述的方法中，第一次指派給`i`特別未參考外部範圍中宣告的欄位。 相反地，本機變數所指的是，則會導致編譯時期錯誤因為賦予在前面的變數宣告。 在 `G`方法中，使用`j`中宣告的初始設定式`j`是有效的因為前面使用沒有*local_variable_declarator*。 在`H`方法，後續*local_variable_declarator*正確是指在稍早宣告的本機變數*local_variable_declarator*在相同*local_variable_declaration*。

本機變數的範圍規則被設計來保證運算式的內容中使用名稱的意義永遠相同區塊內。 如果本機變數的範圍將僅從其宣告區塊的結尾，在上述範例中，第一次指派會將指派給執行個體變數，然後第二項指派會指派給本機變數，可能會導致稍後會重新排列區塊的陳述式一樣，就會產生編譯時期錯誤。

區塊內的名稱的意義可能有所不同的內容中使用的名稱。 在範例
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
名稱`A`運算式內容中使用參考本機變數`A`來參考類別類型內容中`A`。

### <a name="name-hiding"></a>隱藏名稱

實體的範圍通常包含比實體的宣告空間更多的程式文字。 特別是，實體的範圍可能包含引入新的宣告空間，包含具有相同名稱的實體宣告。 這類宣告會造成原始實體變成***隱藏***。 相反地，實體要***可見***未隱藏時。

透過巢狀結構，和透過繼承的範圍重疊時的範圍重疊時，就會發生名稱隱藏。 隱藏的兩種類型的特性是由下列各節所述。

#### <a name="hiding-through-nesting"></a>透過巢狀的隱藏

隱藏透過巢狀的名稱可能是由於巢狀命名空間或命名空間、 巢狀類別或結構內，因為參數和區域變數宣告的型別中的類型。

在範例
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
內`F`方法中，執行個體變數`i`隱藏區域變數`i`，但內`G`方法，`i`仍然是指執行個體變數。

當內部範圍中的名稱會隱藏外部範圍中的名稱時，它會隱藏該名稱的所有多載的項目。 在範例
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
在呼叫`F(1)`叫用`F`中所宣告`Inner`因為的所有外部的項目`F`內部的宣告會隱藏。 基於相同理由，也就是呼叫`F("Hello")`會導致編譯時期錯誤。

#### <a name="hiding-through-inheritance"></a>經由繼承隱藏

當類別或結構重新宣告繼承自基底類別的名稱時，就會發生經由繼承隱藏的名稱。 這種類型的名稱隱藏採用下列格式之一：

*  常數、 欄位、 屬性、 事件或類別或結構中引入的型別會隱藏所有具有相同名稱的基底類別成員。
*  所有非方法的基底類別成員具有相同的名稱，並使用相同的簽章 （方法名稱和參數計數、 修飾詞和型別） 的所有基底類別方法，則會隱藏在類別或結構中引入的方法。
*  在類別或結構中引入的索引子會隱藏所有具有相同的簽章 （參數計數和型別） 的基底類別索引子。

控管運算子宣告的規則 ([運算子](classes.md#operators)) 將會導致在衍生類別宣告基底類別中的運算子具有相同的簽章的運算子。 因此，運算子永遠不會隱藏另一個。

相對於隱藏外部範圍中的名稱，隱藏繼承的範圍從可存取的名稱，將會導致要報告警告。 在範例
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
deklarace`F`在`Derived`會回報警告。 隱藏繼承的名稱是刻意不發生錯誤，因為這樣會妨礙個別發展的基底類別。 例如，上述這種情況就可能會發生因為較新版`Base`導入`F`未出現在較早版本的類別中的方法。 上述這種情況已發生錯誤，然後分開建立版本的類別庫中的基底類別所做的任何變更，都可能會讓衍生的類別，變成無效。

隱藏繼承的名稱所造成的警告會消除，可以透過使用`new`修飾詞：
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

`new`修飾詞表示`F`在`Derived`是 「 新增 」，而且，它確實能隱藏繼承的成員。

新成員的宣告會隱藏繼承的成員只在新成員的範圍內。

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

在範例中的宣告之上`F`中`Derived`隱藏`F`，繼承自`Base`，但由於新`F`中`Derived`具有私人存取權，其範圍不會延伸到`MoreDerived`. 因此，呼叫`F()`中`MoreDerived.G`有效，且將會叫用`Base.F`。

## <a name="namespace-and-type-names"></a>命名空間和類型名稱

中的數種內容C#程式需要*namespace_name*或是*type_name*來指定。

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

A *namespace_name*是*namespace_or_type_name*參考命名空間。 接下來的解析，如下所述*namespace_or_type_name*的*namespace_name*必須參考的命名空間，或否則會發生編譯時期錯誤。 任何類型引數 ([型別引數](types.md#type-arguments)) 中可以存在*namespace_name* （僅限型別可以有型別引數）。

A *type_name*是*namespace_or_type_name* ，參考型別。 接下來的解析，如下所述*namespace_or_type_name*的*type_name*必須參考型別，否則會發生編譯時期錯誤或。

如果*namespace_or_type_name*限定-別名-成員中所述，是它的意義[命名空間別名限定詞](namespaces.md#namespace-alias-qualifiers)。 否則，請*namespace_or_type_name*有四種形式之一：

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

何處`I`是單一的識別項，`N`是*namespace_or_type_name*並`<A1, ..., Ak>`是選擇性*type_argument_list*。 若未*type_argument_list*會指定，請考慮`k`為零。

意義*namespace_or_type_name*判斷方式如下：

*   如果*namespace_or_type_name*的形式`I`，或格式`I<A1, ..., Ak>`:
    * 如果`K`為零， *namespace_or_type_name*出現在泛型方法宣告中 ([方法](classes.md#methods))，如果宣告包含型別參數 ([類型參數](classes.md#type-parameters)) 名稱 `I`，則*namespace_or_type_name*指的是該型別參數。
    * 否則，如果*namespace_or_type_name*會出現在類型宣告中，然後針對每個執行個體類型 `T`([執行個體類型](classes.md#the-instance-type))，從該類型的執行個體類型宣告，並繼續每個封入類別或結構宣告的執行個體類型 （如果有的話）：
        * 如果`K`是零，宣告`T`包含名稱的型別參數 `I`，然後在*namespace_or_type_name*指的是該型別參數。
        * 否則，如果*namespace_or_type_name*出現在類型宣告的主體內並`T`或其任何基底型別包含具有名稱的巢狀可存取型別 `I`和`K`  型別參數，則*namespace_or_type_name*建構具有指定的型別引數的型別參考。 如果沒有這樣的多個型別，會選取衍生程度較大的型別內宣告的型別。 請注意，非類型成員 （常數、 欄位、 方法、 屬性、 索引子、 運算子、 執行個體建構函式、 解構函式和靜態建構函式） 和具有不同數目的型別參數的型別成員時，會忽略判斷的意義*namespace_or_type_name*。
    * 如果不成功，然後，每個命名空間的前一個步驟 `N`，從命名空間，其中*namespace_or_type_name*發生時，每個封入命名空間 （如果有的話），繼續進行，結尾為全域命名空間，直到找到實體為止，會評估下列步驟：
        * 如果`K`為零並`I`中的命名空間名稱 `N`，然後：
            * 如果位置所在*namespace_or_type_name*就會發生加上命名空間宣告`N`且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，將名稱產生關聯 `I`命名空間或類型，則*namespace_or_type_name*模稜兩可並發生編譯時期錯誤。
            * 否則，請*namespace_or_type_name*名為命名空間是指`I`在`N`。
        * 否則，如果`N`包含可存取的型別具有名稱 `I`並`K` 型別參數，然後：
            * 如果`K`是零，位置所在*namespace_or_type_name*就會發生加上命名空間宣告`N`和命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，將關聯名稱 `I`命名空間或類型，則*namespace_or_type_name*是模稜兩可和編譯時間會發生錯誤。
            * 否則，請*namespace_or_type_name*指的是使用指定的型別引數建構的類型。
        * 否則，如果位置所在*namespace_or_type_name*就會發生加上命名空間宣告`N`:
            * 如果`K`為零，而命名空間宣告包含*extern_alias_directive*或是*using_alias_directive*名稱建立關聯的 `I`與匯入的命名空間或型別，則*namespace_or_type_name*參考到該命名空間或型別。
            * 否則，如果所匯入的命名空間和類型的宣告*using_namespace_directive*s 並*using_alias_directive*的命名空間宣告包含一個可存取的型別具有名稱 `I`並`K` 型別參數，則*namespace_or_type_name*建構具有指定的型別引數的型別參考。
            * 否則，如果所匯入的命名空間和類型的宣告*using_namespace_directive*s 並*using_alias_directive*的命名空間宣告包含一個以上的存取類型具有名稱 `I`並`K` 型別參數，則*namespace_or_type_name*模稜兩可並發生錯誤。
    * 否則，請*namespace_or_type_name*是未定義，而且會發生編譯時期錯誤。
*  否則，請*namespace_or_type_name*的形式`N.I`，或格式`N.I<A1, ..., Ak>`。 `N` 會先解析成*namespace_or_type_name*。 如果解析`N`不成功，就會發生編譯時期錯誤。 否則，請`N.I`或`N.I<A1, ..., Ak>`問題解決之後，如下所示：
    * 如果`K`為零並`N`命名空間是指和`N`包含具有名稱的巢狀命名空間`I`，則*namespace_or_type_name*是指該巢狀命名空間。
    * 否則，如果`N`所參考的命名空間並`N`包含可存取的型別具有名稱 `I`並`K` 型別參數，則*namespace_or_type_name*指的是若要建構使用指定的型別引數的型別。
    * 否則，如果`N`參考 （可能是建構） 的類別或結構類型和`N`或任何其基底類別包含具有名稱的巢狀可存取類型 `I`並`K` 類型參數，則*namespace_or_type_name*建構具有指定的型別引數的型別參考。 如果沒有這樣的多個型別，會選取衍生程度較大的型別內宣告的型別。 請注意，如果的意義`N.I`解析的基底類別規格的一部分會決定`N`然後的直接基底類別`N`會被視為物件 ([基底類別](classes.md#base-classes))。
    * 否則，請`N.I`是無效*namespace_or_type_name*，並發生編譯時期錯誤。

A *namespace_or_type_name*允許參考靜態類別 ([靜態類別](classes.md#static-classes)) 只有當

*  *Namespace_or_type_name*是`T`中*namespace_or_type_name*格式的`T.I`，或
*  *Namespace_or_type_name*是`T`中*typeof_expression* ([引數清單](expressions.md#argument-lists)1) 的形式`typeof(T)`。

### <a name="fully-qualified-names"></a>完整限定的名稱

每個命名空間和型別都***完整限定的名稱***，可唯一識別的命名空間或在所有其他項目之間的型別。 命名空間或型別的完整格式的名稱`N`判斷方式如下：

*  如果`N`成員的完整限定的名稱是全域命名空間中， `N`。
*  否則，其完整的名稱是`S.N`，其中`S`命名空間或型別所在的完整格式名稱`N`宣告。

換句話說，完整的名稱的`N`就是完整的階層式路徑的識別項會導致`N`，並且從全域命名空間。 由於每個命名空間或類型成員必須有唯一的名稱，它會依照命名空間或型別的完整格式的名稱永遠是唯一。

下列範例將說明幾種命名空間和類型，以及其相關聯的完整名稱。
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

C# 採用自動記憶體管理，讓開發人員從手動配置及釋放物件所佔用的記憶體。 自動記憶體管理原則由實作***記憶體回收行程***。 物件的記憶體管理生命週期如下所示：

1. 建立物件時，會為它配置記憶體、 建構函式執行時，和該物件會被視為即時。
2. 如果物件或一部分，無法透過任何可能執行的存取，以外執行解構函式時，物件會被視為不再使用中，並成為適合進行解構。 C# 編譯器和記憶體回收行程可能會選擇分析程式碼，以判斷哪一個物件的參考可能會在未來使用。 比方說，如果在範圍內的區域變數是唯一的現有參考的物件，但永遠不會參考該區域變數中從目前的執行執行的任何可能的接續點程序中，記憶體回收行程可能 （但不是才能） 為不再使用中處理物件。
3. 適合解構物件之後，在稍後時間解構函式 ([解構函式](classes.md#destructors)) （如果有的話） 的執行物件。 在正常情況下，雖然實作特定的 Api 可能會允許覆寫此行為，是一次執行物件的解構函式。
4. 當執行物件的解構函式時，如果透過任何可能的執行，包括解構函式的執行，則無法存取該物件或一部分，該物件會被視為無法存取，而且該物件會變成可進行記憶體回收。
5. 最後，某個時間點之後的物件會變成可進行記憶體回收，記憶體回收行程釋放的記憶體與該物件相關聯。

記憶體回收行程會維護物件使用量的相關資訊，並使用這項資訊以提供記憶體管理決策，例如在重新定位的物件，以及當物件已不再使用中或無法存取，請找出新建立的物件，記憶體中的位置。

假設有記憶體回收行程的其他語言，例如 C# 的設計是要讓記憶體回收行程可能會實作各種不同的記憶體管理原則。 比方說，C# 不需要執行解構函式或物件會收集到的只要他們有資格，或任何特定的順序，或在任何特定的執行緒上，執行解構函式。

記憶體回收行程控制行為，以某種程度上，透過在類別上的靜態方法`System.GC`。 這個類別可用來要求回收，解構函式將會執行 （或未執行） 等等。

因為記憶體回收行程相當大的自由決定何時回收物件，並執行解構函式中的，合格的實作可能會產生不同於下列程式碼所示的輸出。 程式
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
建立類別的執行個體`A`類別的執行個體和`B`。 這些物件有資格使用記憶體回收時變數`b`會將值指派給`null`，因為在此時間之後就無法存取它們的任何使用者撰寫程式碼。 輸出可以是
```
Destruct instance of A
Destruct instance of B
```
或
```
Destruct instance of B
Destruct instance of A
```
因為語言會施加在其中物件被記憶體回收的順序沒有限制。

在難以察覺的情況下，「 適合進行解構"和"可進行記憶體回收 」 之間的差別可能很重要。 例如，套用至物件的
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

在上述程式中，如果選擇要執行的解構函式的記憶體回收行程`A`的解構函式之前`B`，則可能是此程式的輸出：
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

請注意，雖然執行個體`A`無法在使用中和`A`的執行解構函式、 方法仍可能`A`(在此情況下， `F`) 從另一個解構函式呼叫。 另請注意執行解構函式可能會導致要再次變成主線程式可用的物件。 在此情況下，執行`B`的解構函式造成的執行個體`A`，先前在非使用可從即時參考`Test.RefA`。 在呼叫之後`WaitForPendingFinalizers`，執行個體`B`符合資格的集合，但執行個體`A`不是，因為參考`Test.RefA`。

為了避免混淆和非預期的行為，它通常是個不錯的主意的解構函式只儲存在其物件自己的欄位中，資料上執行清除作業，而非參考的物件或靜態欄位上執行任何動作。

使用解構函式的替代方式是讓實作類別`System.IDisposable`介面。 這可讓用戶端的物件，判斷何時要釋放物件的資源通常為中的資源存取物件`using`陳述式 ([using 陳述式](statements.md#the-using-statement))。

## <a name="execution-order"></a>執行順序

C# 程式的執行程序，每個執行中執行緒的副作用會保留在重要的執行時間點。 A***副作用***定義為讀取或寫入 volatile 欄位，寫入靜態變數時，寫入外部資源，並擲回例外狀況。 這些副作用的順序必須保留在其中的關鍵執行點是變動性的欄位參考 ([Volatile 欄位](classes.md#volatile-fields))，`lock`陳述式 ([lock 陳述式](statements.md#the-lock-statement))，以及執行緒建立與終止。 執行環境是可用來變更執行的 C# 程式，受限於下列條件約束的順序：

*  資料相依性會保留在執行的執行緒。 亦即，如同原始依照程式順序所執行的執行緒中的所有陳述式，會計算每個變數的值。
*  初始設定順序保留規則 ([欄位初始化](classes.md#field-initialization)並[區域變數初始設定式](classes.md#variable-initializers))。
*  變動性的讀取和寫入方面保留的副作用的順序 ([Volatile 欄位](classes.md#volatile-fields))。 此外，如果它可以推斷不會使用該運算式的值和任何必要的副作用所產生 （包括任何因呼叫方法或存取 volatile 欄位） 的執行環境需要無法評估運算式的一部分。 當程式執行中斷的非同步事件 （例如另一個執行緒所擲回例外狀況） 時，不保證可預見的副作用會顯示在原始程式的順序。
