# 類別及結構

- [類別與結構的比較](#comparing)
- [類別與結構實體](#instance)
- [取得屬性](#property)
- [結構型別的成員逐一建構器](#memberwise_initializer)
- [值型別與參考型別](#value_type_and_reference_type)
- [恆等運算子](#identity_operator)
- [選擇使用類別或結構](#choosing)

在前面章節介紹了[函式](../ch1/functions.md)，它是一段執行特定任務的獨立程式碼區塊，而更進階地，Swift 提供了兩個型別語法：類別(`class`)及結構(`structure`)，可以讓你將多個相關的函式及值儲存在內，以及更多的特性。

<a name="comparing"></a>
### 類別與結構的比較

類別及結構有很多相同的地方，如下：

- [屬性](../ch2/properties.md)(`property`)：用於儲存值
- [方法](../ch2/methods.md)(`method`)：用於提供功能
- [下標](../ch2/subscripts.md)(`subscript`)：用於存取值
- [建構器](../ch2/initialization_deinitialization.md#initializer)(`initializer`)：用於生成初始化值
- [擴展](../ch2/extensions.md)(`extension`)：增加預設實作的功能
- [協定](../ch2/protocols.md)(`protocol`)：對某類別提供標準功能

與結構相比，**類別**還有以下的其他功能：

- [繼承](../ch2/inheritance.md)(`inherit`)：類別可以繼承另一個類別的內容
- [解構器](../ch2/initialization_deinitialization.md#deinitializer)(`deinitializer`)允許一個類別實體釋放任何其所被分配的資源
- [型別轉換](../ch2/type-casting.md)允許在執行時檢查和轉換一個類別實體的型別
- [參考計數](../ch2/arc.md)允許對一個類別實體的多次參考

以上這些特性，會在後面章節陸續介紹。

定義一個類別及結構分別要使用`class`及`struct`關鍵字，並接著一組大括號`{}`，格式如下：

```swift
class 類別名稱 {
    類別內的屬性、方法及其他可以定義在內的特性
}

struct 結構名稱 {
    結構內的屬性、方法及其他可以定義在內的特性
}

```

##### Hint

- 類別或結構內的變數或常數，會稱作屬性(`property`)。而類別或結構內的函式，會稱作方法(`method`)。
- 每次定義一個新的類別或結構時，實際上你是定義了一個新的 Swift 型別，所以在習慣上會以[大駝峰式命名法](../more/camel_case_naming.md#upper)來為類別與結構命名，以符合標準 Swift 型別的大寫命名風格(像是`String`、`Int`)。相對地，使用[小駝峰式命名法](../more/camel_case_naming.md#lower)為屬性與方法命名(與常數、變數及函式相同)，以便與類別區分。

以下是定義結構與類別的例子：

```swift
struct CharacterStats {
    var hp = 0.0
    var mp = 0.0
}

class GameCharacter {
    var stats = CharacterStats()
    var attackSpeed = 1.0
    var name: String?
}

```

上述程式中，定義了一個名為`CharacterStats`的結構來描述一個遊戲角色的狀態，這個結構包含了兩個屬性(屬性即為儲存在類別或結構內的常數或變數)：`hp`跟`mp`，分別表示角色血量與法力的最大值。當這兩個屬性被初始化為`0.0`時，會被自動推斷為`Double`型別。

接著定義了一個名為`GameCharacter`的類別，描述一個遊戲角色的基本資訊，這個類別包含了三個屬性，第一個為角色的狀態，會被初始化為一個`CharacterStats`結構的實體，其後兩個屬性為攻速及名字，攻速有預設值`1.0`，名字則為一個可選型別`String?`，會被自動指派為一個預設值`nil`，表示一開始沒有`name`值。

<a name="instance"></a>
### 類別與結構實體

定義類別與結構，就像是給了一個固定的規格，譬如說明一個遊戲角色會有血量、法力、攻速、名字這些資訊，但使用上，必須依照這個規格(即類別或結構)來實際建立一個角色，這個動作就是生成類別或結構的**實體(instance)**。

##### Hint

- 實體有些地方會翻譯成**實例**，同樣都是指`instance`。

類別與結構都使用建構器語法來生成實體，建構器最簡單的形式就是在類別或結構名稱後面加上一個小括號`()`語法，這種方式建立的實體，其內的屬性都會被初始化為預設值，如下：

```swift
let someStats = CharacterStats()
let someGameCharacter = GameCharacter()

```

上述程式中，`someStats`就是一個`CharacterStats`結構的實體，`someGameCharacter`則為一個`GameCharacter`類別的實體。

後面章節會正式介紹[建構器](../ch2/initialization_deinitialization.md#initializer)。

可以看到生成實體的方式與函式相似，所以習慣上兩種命名方式會不一樣，一個使用[大駝峰式命名法](../more/camel_case_naming.md#upper)，另一個使用[小駝峰式命名法](../more/camel_case_naming.md#lower)，來作區別。

<a name="property"></a>
### 取得屬性

使用**點語法**(`dot syntax`)可以取得實體的**屬性**(`property`)。規則為實體名稱後加上一個點號`.`，然後緊接著屬性名稱，如下：

```swift
// 這邊使用前面定義的 CharacterStats 結構 及生成的實體 someStats
// 因為沒有初始值 所以會使用預設值 這邊會印出：someStats 血量最大值為0.0
print("someStats 血量最大值為\(someStats.hp)")

```

也可以直接取得子屬性，像是`someGameCharacter`中`stats`屬性的`hp`屬性，如下：

```swift
// 這邊使用前面定義的 GameCharacter 類別
// 及其生成的實體 someGameCharacter
// 印出：血量最大值為0.0
print("血量最大值為\(someGameCharacter.stats.hp)")

```

也可以將新的值指派給實體的屬性，如下：

```swift
// 將 someGameCharacter 的血量最大值指派為 500
someGameCharacter.stats.hp = 500
// 再次印出 即會變成：500.0
print(someGameCharacter.stats.hp)

```

<a name="memberwise_initializer"></a>
### 結構型別的成員逐一建構器

所有結構都有一個自動生成的**成員逐一建構器**(`memberwise initializer`)，當要生成一個結構的實體時，用來初始化實體內的屬性，如下：

```swift
// 這邊使用前面定義的 CharacterStats 結構 生成新的實體 someoneStats
let someoneStats = CharacterStats(hp: 120, mp: 100)

// 印出：120.0
print(someoneStats.hp)

```

##### Hint

- 類別實體沒有成員逐一建構器這個功能。

<a name="value_type_and_reference_type"></a>
### 值型別與參考型別

Swift 中以記憶體配置的方式不同來說，可以分為值型別(`value type`)與參考型別(`reference type`)。**值型別**會儲存實際的值，而**參考型別**只是儲存其在記憶體空間中配置的位置。

#### 結構和列舉是值型別

**值型別**(`value type`)在被指派給一個變數、常數或被傳遞給一個函式時，實際上操作的是其拷貝(copy)。指派或傳遞後，兩者的值即各自獨立，不會互相影響。

在前面章節中，其實已經大量使用了值型別。實際上，在 Swift 中，所有的基本型別：整數、浮點數、布林值、字串、陣列和字典，都是值型別，並且背後都是以結構的形式實作。

結構和列舉也都是值型別，代表它們的實體及其內任何值型別的屬性，在被指派或傳遞時都會被複製。例子如下：

```swift
// 這邊使用前面定義的 CharacterStats 結構
var oneStats = CharacterStats(hp: 120, mp: 100)
var anotherStats = oneStats

// 這時修改 anotherStats 的 hp 屬性
anotherStats.hp = 300
// 可以看出來已經改變了 印出：300.0
print(anotherStats.hp)

// 但 oneStats 的屬性不會改變
// 仍然是被生成實體時的初始值 印出：120.0
print(oneStats.hp)

```

在`oneStats`被指派給`anotherStats`時，其實是將`oneStats`內的值進行拷貝(`copy`)，接著將拷貝的資料儲存給新的實體`anotherStats`，兩者是完全獨立的實體，彼此不會互相影響。

##### Hint

- 雖然值型別的指派或傳遞是一個「拷貝」的動作，在程式碼中會大量的出現，但 Swift 在處理時只有在確實必要時才會執行實際的拷貝，系統會管理這部份的最優化性能，所以沒有必要為了性能特地避免指派值。


#### 類別是參考型別

與值型別不同，參考型別(`reference type`)在被指派給一個變數、常數或被傳遞給一個函式時，操作的不是其拷貝，而是已存在的實體本身。例子如下：

```swift
// 這邊使用前面定義的 GameCharacter 類別
let archer = GameCharacter()
archer.attackSpeed = 1.5
archer.name = "弓箭手"

// 指派給一個新的常數
let superArcher = archer
// 並修改這個新實體的屬性 attackSpeed
superArcher.attackSpeed = 1.8

// 可以看到這邊印出的都為：1.8
print("archer 的攻速為 \(archer.attackSpeed)")
print("superArcher 的攻速為 \(superArcher.attackSpeed)")

```

從上述程式可以知道，`archer`跟`superArcher`實際上是對同一個實體做操作。也就是同一個實體的兩個不同名稱。

有一點要注意的是，可以看到`archer`跟`superArcher`都被宣告為常數(使用`let`宣告)，但依然可以改變其內的屬性`archer.attackSpeed`跟`superArcher.attackSpeed`，因為這兩個常數儲存的是一個`GameCharacter`類別實體的參考(參考其在記憶體空間內配置的位置)，而不是儲存這個實體，所以這個實體的`attackSpeed`屬性可以被修改，但不會修改到常數的值。

<a name="identity_operator"></a>
### 恆等運算子

因為類別是參考型別，可能有多個常數和變數在後台同時參考某一個類別實體，所以 Swift 提供了兩個恆等運算子，能夠判斷兩個變數或常數是不是參考同一個類別實體：

- 等價於 `===`
- 不等價於 `!==`

以下是一個例子：

```swift
// 使用前面宣告的兩個常數 archer 與 superArcher
if archer === superArcher {
    print("沒錯！！！是同一個類別實體")
}

```

##### Hint

- 請注意`===`與`==`的不同
  - 等價於(`===`)為判斷兩個變數或常數是不是參考同一個類別實體
  - 等於(`==`)為判斷兩個值是否相等

<a name="choosing"></a>
### 選擇使用類別或結構

類別與結構有很多相似的地方，要如何決定每個資料建構要定義為類別還是結構，可以參考以下原則：

- 需要封裝的資料量較少且較簡單。
- 在指派或傳遞這個實體時，有特別需求這個資料是會被拷貝而不是參考。
- 不需要去繼承另一個已存在型別的屬性或行為。

如果符合上述一條或以上原則，則建議定義為結構，其餘則是定義為類別。

所以實際上，大部分的自定義資料建構都應該使用類別。


### 範例

本節範例程式碼放在 [ch2/classes_structures.playground](https://github.com/itisjoe/swiftgo_files/tree/master/ch2/classes_structures.playground)

