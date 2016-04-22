# 協定

- [協定語法](#protocol)
- [屬性的規則](#property)
- [方法的規則](#method)
- [建構器的規則](#initializer)
- [協定為一種型別](#protocol_is_a_type)
- [委任模式](#delegation)
- [為擴展添加協定](#adding_protocol_conformance_with_an_extension)
- [協定型別的集合](#collection)
- [協定的繼承](#inheritance)
- [協定合成](#composition)
- [檢查協定](#check)
- [可選協定的規則](#optional_protocol)
- [協定擴展](#extension)

協定(`protocol`)是 Swift 一個重要的特性，它會定義出為了**完成某項任務或功能**所需的方法、屬性，協定本身不會實作這些任務跟功能，而僅僅只是表達出該任務或功能的名稱。這些功能則都交由**遵循**協定的型別來實作，列舉、結構及類別都可以遵循協定，遵循協定表示這個型別必須實作出協定定義的方法、屬性或其他功能。

有點像是協定定義出一個`To Do List`，而所有遵循協定的型別都必須照表操課，將需要的功能都實作出來。

<a name="protocol"></a>
### 協定語法

使用`protocol`關鍵字來定義一個協定，格式如下：

```swift
protocol 協定名稱 {
    協定定義的內容
}

```

要讓自定義的型別遵循協定，寫法有點像繼承，一樣是把協定名稱寫在冒號`:`後面，而要遵循多個協定時，則是以逗號`,`分隔每個協定，格式如下：

```swift
struct 自定義的結構名稱: 協定, 另一個協定 {
    結構的內容
}

```

同時要繼承父類別跟遵循協定時，應該將父類別名稱寫在第一個，其後才是接著協定名稱，同樣都是以逗號`,`分隔，格式如下：

```swift
class 類別名稱: 父類別, 協定, 另一個協定 {
    類別的內容
}

```

<a name="property"></a>
### 屬性的規則

協定不能定義一個屬性是儲存屬性或計算屬性，而只是定義屬性的**名稱**及**是實體屬性或型別屬性**。此外還可以定義屬性是**唯讀**或是**可讀寫**的。

-  協定定義屬性是可讀寫時，則遵循協定的型別定義的屬性不能是常數屬性或唯讀的計算屬性。
-  協定定義屬性是唯讀時，則遵循協定的型別定義的屬性可以是唯讀或依照需求改定義為可讀寫。

協定使用`var`關鍵字來定義變數屬性，在型別標註後加上`{ get set }`來表示是可讀寫的，唯讀則是使用`{ get }`來表示，如下：

```swift
protocol 協定 {
    var 可讀寫變數: 型別 { get set }
    var 唯讀變數: 型別 { get }
}

```

在協定中定義型別屬性時，必須在前面加上`static`關鍵字。而當一個類別遵循這個協定時，除了`static`還可以使用`class`關鍵字來定義類別的這個型別屬性：

```swift
protocol 協定 {
    static var 型別屬性: 屬性型別 { get set }
}

```

底下是一個例子：

```swift
// 定義一個協定 包含一個唯讀的字串屬性
protocol FullyNamed {
    var fullName: String { get }
}

// 定義一個類別 遵循協定 FullyNamed
struct Person: FullyNamed {
    // 因為遵循協定 FullyNamed
    // fullName 這個屬性一定要定義才行 否則會報錯誤
    var fullName: String
}

let joe = Person(fullName: "Joe Black")
print("名字為 \(joe.fullName)")
// 印出：名字為 Joe Black

```

<a name="method"></a>
### 方法的規則

協定可以定義實體方法或型別方法以供遵循，而這些方法不需要大括號`{}`以及其內的內容(即不需要實作)，而實作則是交給遵循協定的型別來做。

- 協定可以定義含有可變數量參數(`variadic parameter`)的方法。
- 協定不能為方法的參數提供預設值。

與屬性的規則一樣，協定中要定義型別方法時，必須在前面加上`static`關鍵字。而當一個類別遵循這個協定時，除了`static`還可以使用`class`關鍵字來定義類別的這個型別方法。

協定定義方法的格式如下：

```swift
protocol SomeProtocol {
    // 定義一個型別方法
    static func someTypeMethod()
    
    // 定義一個實體方法
    func instanceMethod() -> Double

    // 協定定義方法皆不需要大括號 {} 及其內內容
}

```

底下是一個例子：

```swift
protocol SomeProtocol {
    // 定義一個實體方法 返回一個整數
    func instanceMethod() -> Int
}

// 定義一個類別 遵循協定 SomeProtocol
class MyClass: SomeProtocol {
    // 因為遵循協定 SomeProtocol
    // instanceMethod() 這個方法一定要定義才行 否則會報錯誤
    func instanceMethod() -> Int {
        return 300
    }
}

```

#### 變異方法的規則

使用`mutating`關鍵字放在`func`關鍵字前來定義變異方法(變異方法表示可以在方法中修改它所屬的實體以及實體的屬性的值)。

遵循一個包含變異方法的協定時，列舉跟結構定義時必須加上`mutating`關鍵字，而類別定義時則不用加上。

底下是一個例子：

```swift
// 定義一個包含變異方法的協定
protocol Togglable {
    // 只需標明方法名稱 不用實作
    mutating func toggle()
}

// 定義一個開關切換的列舉
enum OnOffSwitch: Togglable {
    case Off, On

    // 實作這個遵循協定後需要定義的變異方法
    mutating func toggle() {
        // 會在 On, Off 兩者中切換
        switch self {
        case Off:
            self = On
        case On:
            self = Off
        }
    }
}

var lightSwitch = OnOffSwitch.Off
lightSwitch.toggle()
// lightSwitch 現在切換為 .On

```

<a name="initializer"></a>
### 建構器的規則

協定可以定義一個建構器，與定義方法一樣不需要寫大括號`{}`及其內的內容，格式如下：

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}

```

#### 類別中實作協定的建構器

如果是一個類別遵循一個含有建構器的協定時，無論是指定建構器或便利建構器，都必須為類別的建構器加上`required`修飾符，以確保所有子類別也必須定義這個建構器，從而符合協定(如果類別已被加上`final`，則不需要為其內的建構器加上`required`，因為`final`類別不能再被子類別繼承)，如下：

```swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // 建構器的內容
    }
}

```

如果一個子類別覆寫了父類別的指定建構器，且此建構器滿足了某個協定的要求，則該建構器必須同時加上`required`和`override`，如下：

```swift
// 定義一個協定
protocol SomeProtocol {
    init()
}

// 定義一個類別
class SomeSuperClass {
    init() {
        // 建構器的內容
    }
}

// 定義一個繼承 SomeSuperClass 的類別 同時還遵循了協定 SomeProtocol
class SomeSubClass: SomeSuperClass, SomeProtocol {
    // 必須同時加上 required 和 override
    required override init() {
        // 建構器的內容
    }
}

```

#### 可失敗建構器的規則

協定可以定義可失敗建構器：

- 一個協定包含**可失敗建構器**時，遵循這個協定的型別，可以使用**可失敗建構器**(`init?`)或**非可失敗建構器**(`init`)來定義這個建構器。
- 一個協定包含**非可失敗建構器**時，遵循這個協定的型別，可以使用**隱式解析可失敗建構器**(`init!`)或**非可失敗建構器**(`init`)來定義這個建構器。

<a name="protocol_is_a_type"></a>
### 協定為一種型別

雖然協定本身沒有實作任何功能，但協定仍可以被當做一種型別來使用。使用情況就如同一般型別一樣，如下：

- 作為函式、方法或建構器的參數型別或返回值型別。
- 作為常數、變數或屬性的型別。
- 作為陣列、字典或其他集合中的元素型別。

協定習慣的命名方式就如同一般型別一樣使用大寫字母開頭的大駝峰式寫法。

底下為一個例子：

```swift
// 定義一個協定
protocol SomeProtocol {
    func method() -> Int
}

// 定義一個類別 遵循協定 SomeProtocol
class OneClass: SomeProtocol {
    func method() -> Int {
        return 5566
    }
}

// 定義另一個類別 有一個型別為 SomeProtocol 的常數
class AnotherClass {
    // 常數屬性 型別為[協定 SomeProtocol]
    let oneMember: SomeProtocol
    
    // 建構器有個參數 member 型別為 SomeProtocol
    init(member: SomeProtocol) {
        self.oneMember = member
    }
    
}

// 先宣告一個類別 OneClass 的實體
let oneInstance = OneClass();

// 任何遵循[協定 SomeProtocol]的實體 都可以被當做[協定 SomeProtocol]型別
// 所以上面宣告的 oneInstance 可以被當做參數傳入
let twoInstance = AnotherClass(member: oneInstance)

// 印出：5566
print(twoInstance.oneMember.method())

```

由上述程式可以知道，任何遵循一個協定的實體，都可以被當做這個協定型別的值。

<a name="delegation"></a>
### 委任模式

委任(`delegation`)是一種設計模式，它允許類別或結構將一些需要它們負責的功能委任給其他型別的實體。

委任模式的實作就是定義協定來封裝那些需要被委任的功能，而遵循這個協定的型別就能提供這些功能。委任模式可以用來回應特定的動作或是接收外部資料，而不需要知道外部資料的型別。

以下是一個例子：

```swift
// 定義一個協定 遵循這個協定的類別都要實作 attack() 方法
protocol GameCharacterProtocol {
    func attack()
}

// 定義一個委任協定 將一些其他功能委任給別的實體實作
protocol GameCharacterProtocolDelegate {
    // 這邊是定義一個在攻擊後需要做的整理工作
    func didAttackDelegate()
}

// 定義一個類別 表示一個遊戲角色 
class GameCharacter: GameCharacterProtocol {
    // 首先定義一個變數屬性 delegate 型別為 GameCharacterProtocolDelegate
    // 定義為可選型別 會先初始化為 nil 之後再將其設置為負責其他動作的另一個型別的實體
    var delegate: GameCharacterProtocolDelegate?

    // 因為遵循[協定 GameCharacterProtocol]
    // 所以需要實作 attack() 這個方法
    func attack() {
        print("攻擊！")

        // 最後將其他動作委任給另一個型別的實體實作
        delegate?.didAttackDelegate()
    }
}

// 定義一個類別 遵循[協定 GameCharacterProtocolDelegate]
// 這個類別生成的實體會被委任其他動作
class GameCharacterDelegate: GameCharacterProtocolDelegate {
    // 必須實作這個方法
    func didAttackDelegate() {
        print("攻擊後的整理工作")
    }
}

// 首先生成一個遊戲角色的實體
let oneChar = GameCharacter()

// 接著生成一個委任類別的實體 要負責其他的動作
let charDelegate = GameCharacterDelegate()

// 將遊戲角色的 delegate 屬性設為委任的實體
oneChar.delegate = charDelegate

// 接著呼叫攻擊方法
oneChar.attack()
// 會依序印出：
// 攻擊！
// 攻擊後的整理工作

```

<a name="adding_protocol_conformance_with_an_extension"></a>
### 為擴展添加協定

你也可以讓擴展遵循協定，這樣就可以在不修改原始程式碼的情況下，讓已存在的型別經由擴展來遵循一個協定。當已存在型別經由擴展遵循協定時，這個型別的所有實體也會隨之獲得協定中定義的功能。

```swift
// 定義另一個協定 增加一個防禦方法 defend
protocol GameCharacterDefend {
    func defend()
}

// 定義一個擴展 會遵循新定義的協定 GameCharacterDefend
extension GameCharacter: GameCharacterDefend {
    // 必須實作這個方法
    func defend() {
        print("防禦！")
    }
}

// 使用前面生成的實體 oneChar
// 這樣這個被擴展的類別生成的實體 也隨即可以使用這個方法
oneChar.defend()
// 印出：防禦！

```

#### 經由擴展遵循協定

當一個型別已經符合某個協定的所有要求，但卻沒有在型別的定義中宣告時，可以經由一個空的擴展來遵循這個協定，以下是個例子：

```swift
// 定義一個協定
protocol SomeProtocol {
    var name: String { get set }
}

// 定義一個類別 滿足了[協定 SomeProtocol]的要求 但尚未遵循它
class SomeClass {
    var name = "good day"
}

// 這時可以使用擴展來遵循
extension SomeClass: SomeProtocol {}

```

##### Hint

- 即使滿足了協定的所有要求，型別也不會自動遵循協定，必須顯式地為它加上遵循協定才行。

<a name="collection"></a>
### 協定型別的集合

協定型別也可以作為陣列或字典內成員的型別，以下是個例子：

```swift
// 生成另外兩個實體
let twoChar = GameCharacter()
let threeChar = GameCharacter()

// 宣告一個型別為 [GameCharacterProtocol] 的陣列
let team: [GameCharacterProtocol] = [oneChar, twoChar, threeChar]

// 因為都遵循這個協定 所以這個 attack() 方法一定存在可以呼叫
for member in team {
    member.attack()
}

```

<a name="inheritance"></a>
### 協定的繼承

協定也可以像類別一樣繼承另外一個或多個協定，可以在繼承的協定的基礎上增加新的功能，繼承的多個協定一樣是使用逗號`,`隔開，如下：

```swift
protocol 新協定: 繼承的協定, 繼承的另一個協定 {
    協定新增加的功能
}

```

#### 只用於類別的協定

你可以在協定的繼承列表中，增加關鍵字`class`來限制這個協定只能被類別型別遵循，而列舉跟結構不能遵循這個協定。`class`必須擺在繼承列表的第一個，在其他要繼承的協定之前，如下：

```swift
protocol 只用於類別的協定: class, 其他要遵循的協定 {
    只用於類別的協定的功能
}

```

<a name="composition"></a>
### 協定合成

如果你需要一個型別同時遵循多個協定，你可以將多個協定使用`protocol<SomeProtocol, AnotherProtocol>`這種格式組合起來，這種方式稱為**協定合成**(`protocol composition`)，在`<>`中可以填入多個需要遵循的協定，並以逗號`,`隔開，如下：

```swift
// 定義一個協定 有一個 name 屬性
protocol Named {
    var name: String { get }
}

// 定義另一個協定 有一個 age 屬性
protocol Aged {
    var age: Int { get }
}

// 定義一個結構 遵循上面兩個定義的協定
struct Person: Named, Aged {
    var name: String
    var age: Int
}

// 定義一個函式 有一個參數 定義為遵循這兩個協定的型別
// 所以寫成 protocol<Named, Aged> 格式
func wishHappyBirthday(celebrator: protocol<Named, Aged>) {
    print("生日快樂！ \(celebrator.name) ， \(celebrator.age) 歲囉！")
}

let birthdayPerson = Person(name: "Brian", age: 25)
wishHappyBirthday(birthdayPerson)
// 印出：生日快樂！ Brian ， 25 歲囉！

```

<a name="check"></a>
### 檢查協定

你可以使用前面章節提過的`is`與`as`來檢查是否符合某協定或是轉換到指定的協定型別，使用方式與型別檢查與轉換一樣：

- `is`用來檢查實體是否符合某協定，符合會返回`true`，反之則返回`false`。
- `as?`返回一個可選值。當實體符合某協定時，會返回協定型別的可選值，反之則返回`nil`。
- `as!`將實體強制向下轉換到某協定型別，如果失敗則會引發運行時錯誤。

以下是一個例子：

```swift
// 定義一個協定 有一個 area 屬性 表示面積
protocol HasArea {
    var area: Double { get }
}

// 定義一個圓的類別 遵循[協定 HasArea] 所以會有 area 屬性
class Circle: HasArea {
    var area: Double
    init(radius: Double) { self.area = 3.14 * radius * radius }
}

// 定義一個國家的類別 遵循[協定 HasArea] 所以會有 area 屬性
class Country: HasArea {
    var area: Double
    init(area: Double) { self.area = area }
}

// 定義一個動物的類別 沒有面積
class Animal {
    var legs: Int
    init(legs: Int) { self.legs = legs }
}

// 以上三個類別的實體都可以作為 [AnyObject] 陣列的成員
let objects: [AnyObject] = [
    Country(area: 243610),
    Circle(radius: 2.0),
    Animal(legs: 4)
]

// 遍歷這個陣列
for object in objects {
    // 使用可選綁定來將成員綁定為 HasArea 的實體
    if let objectWithArea = object as? HasArea {
        // 符合協定 就會綁定成功 也就可以取得 area 屬性
        print("面積為 \(objectWithArea.area)")
    } else {
        // 不符合協定 則是返回 nil
        print("沒有面積！")
    }
}

// 依序印出：
// 面積為 243610.0
// 面積為 12.56
// 沒有面積！

```

<a name="optional_protocol"></a>
### 可選協定的規則

你可以在`protocol`前面加上`@objc`特性來讓協定可以定義它的功能(像是屬性或方法)為可選。要定義一個可選功能，必須在前面加上`optional`關鍵字。

如果將功能變為可選後，它們的型別會自動變成可選的。像是一個型別為`(Int) -> String`的方法會變成`((Int) -> String)?`，是**方法的型別**為可選，不是方法返回值的型別。

而這些定義為可選的功能，可以使用**可選鏈**來呼叫。

以下是一個例子：

```swift
// 要加上 @objc 必須引入 Foundation
import Foundation
// 這邊不詳細說明 因為可選協定與 Objective-C 程式語言有關係
// 而 Objective-C 大量使用到 Foundation 的功能 所以需要引入

// 定義一個可選協定 用於計數 分別有兩種不同的增量值
@objc protocol CounterDataSource {
    // 定義一個可選方法 可以傳入一個要增加的整數
    optional func incrementForCount(count: Int) -> Int
    
    // 定義一個可選屬性 為一個固定增加的整數
    optional var fixedIncrement: Int { get }
}

// 定義一個遵循可選協定的類別 計數用
class CounterSource: CounterDataSource {
    // 一個經由遵循協定而擁有的可選屬性 設值為 3
    // 前面必須加上 @objc
    @objc let fixedIncrement = 3
    
    // 不過因為是可選的 所以另一個可選方法可以不用實作 這邊將其註解起來
    /*
    @objc func incrementForCount(count: Int) -> Int {
        return count
    }
    */

}

// 用來計數的變數
var count = 0

// 生成一個型別為[可選協定 CounterDataSource]的實體
// 因為類別 CounterSource 有遵循這個協定 所以可以指派為這個類別的實體
var dataSource: CounterDataSource = CounterSource()

// 迴圈跑 4 次
for _ in 1...4 {
    // 使用可選綁定
    // 首先呼叫 incrementForCount() 方法 但因為這是個可選方法 所以需要加上 ?
    // 而目前這個 incrementForCount 沒有實作這個方法
    // 所以會返回 nil 也就不會執行 if 內的程式
    if let amount = dataSource.incrementForCount?(count){
        count += amount
    }
    // 接著依舊使用可選綁定 取得屬性 fixedIncrement
    // 因為有設置這個屬性 所以會有值 流程則會進入此 else if 內的程式
    else if let amount = dataSource.fixedIncrement{
        count += amount
    }
}

// 因為迴圈跑了 4 次,每次都是加上 3 ,所以最後計為 12
// 印出：最後計數為 12
print("最後計數為 \(count)")

```

<a name="extension"></a>
### 協定擴展

你也可以擴展一個協定來為遵循這個協定的型別新增屬性、方法或下標，而不需要在每個遵循這個協定的型別內實作一樣的功能。以下是個例子：

```swift
// 擴展前面定義的協定 GameCharacterProtocol
// 此協定原本只有定義一個 attack() 方法
// 這邊增加一個新的方法
extension GameCharacterProtocol {
    func superAttack() {
        print("額外的攻擊！")
        attack()
    }
}

// 生成一個遊戲角色的實體
let member = GameCharacter()

// 可以直接呼叫擴展協定後新增的方法
member.superAttack()
// 依序印出：
// 額外的攻擊！
// 攻擊！
// 攻擊後的整理工作

```

由上述程式可知，經由擴展一個協定，可以直接為屬性、方法及下標建立預設的實作功能，而這些遵循協定的型別如果自己又另外實作的話，則這些自定義的實作會替代擴展中的預設實作功能。

#### 為協定擴展添加限制條件

在擴展一個協定時，可以指定一些限制條件，當遵循協定的型別滿足這些限制條件時，才能獲得這個擴展的協定提供的預設實作。使用方式為在協定名稱後面加上`where`語句並接著限制條件。例子如下：

```swift
// 先為[協定 GameCharacterProtocol]經由擴展增加一個新的屬性
extension GameCharacterProtocol {
    var description: String {
        return "成員"
    }
}

// 接著擴展[集合型別的協定 CollectionType] 且其內成員必須遵循[協定 GameCharacterProtocol]
extension CollectionType where Generator.Element: GameCharacterProtocol {
    var allDescription: String {
        let itemsAsText = self.map { $0.description }
        return "[" + itemsAsText.joinWithSeparator(", ") + "]"
    }
}

// 生成三個實體並放入一個陣列中
let oneMember = GameCharacter()
let twoMember = GameCharacter()
let threeMember = GameCharacter()
let myTeam = [oneMember, twoMember, threeMember]

// 因為陣列定義時有遵循[協定 CollectionType]
// 且其內成員都遵循[協定 GameCharacterProtocol]
// 所以這個 allDescription 屬性會自動獲得
// 印出：[成員, 成員, 成員]
print(myTeam.allDescription)

```

