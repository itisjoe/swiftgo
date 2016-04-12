# 協定

協定(`protocol`)是 Swift 一個重要的特性，它會定義出為了完成某項任務或功能所需的方法、屬性，協定本身不會實作這些任務跟功能，而僅僅只是表達出該任務或功能的名稱。

這些功能都交由**遵循**(`adopt`)協定的型別來實作，列舉、結構及類別都可以遵循協定，遵循協定表示這個型別必須實作出協定定義的方法、屬性或其他功能。

有點像是協定定義出一個`To Do List`，而所有遵循協定的型別都必須照表操課，將需要的功能都實作出來。

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


### 方法的規則

協定可以定義實體方法或型別方法以供遵循，而這些方法不需要大括號`{}`以及其內的內容(即不需要實作)，而實作則是交給遵循協定的型別來做。

- 協定可以定義含有可變參數(`variadic parameter`)的方法。
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

print(twoInstance.oneMember.method())
// 印出：5566

```

由上述程式可以知道，任何遵循一個協定的實體，都可以被當做這個協定型別的值。


### 委任模式


### 為擴展添加協定


### 協定型別的集合


### 協定的繼承


### 只用於類別的協定


### 協定合成

協定合成(`protocol composition`)


### 檢查協定


### 可選協定的規則


### 協定擴展

#### 為協定提供預設的實作

#### 為協定擴展添加限制條件
















