# 自動參考計數

- [自動參考計數的運作方式](#arc)
- [類別實體間的強參考循環](#strong_reference_cycles)
- [解決實體間的強參考循環](#resolving_strong_reference_cycles)
- [閉包的強參考循環](#strong_reference_cycles_for_closures)
- [解決閉包的強參考循環](#resolving_strong_reference_cycles_for_closures)

Swift 使用自動參考計數(`ARC`, `Automatic Reference Counting`)機制來追蹤與管理記憶體使用狀況，所以大部分情況下，你不需要自己管理，Swift 會自動釋放掉不需要的記憶體。

##### Hint

- 參考計數只應用在類別(也就是參考型別)的實體。結構與列舉為值型別，也不是通過參考的方式儲存與傳遞。

<a name="arc"></a>
### 自動參考計數的運作方式

當一個類別實體被指派值(給一個屬性、常數或變數)的時候，會建立一個該實體的**強參考**(`strong reference`)，同時會將**參考計數**(`reference counting`)加 1 ，強參考表示會將這個實體保留住，只要強參考還在(也就是參考計數不為 0 )，儲存這個實體的記憶體就不會被釋放掉。

以下簡單介紹一下 **ARC** 運作的方式：

```swift
// 定義一個類別 SomePerson
class SomePerson {
    let name: String
    init (name: String) {
        self.name = name
    }
}

// 先宣告三個可選 SomePerson 的變數 會被自動初始化為 nil
// 這三個變數目前都尚未有實體的參考
var reference1: SomePerson?
var reference2: SomePerson?
var reference3: SomePerson?

// 先生成一個實體 並指派給其中一個變數 reference1
reference1 = SomePerson(name: "Jess")

// 目前這個實體就有了一個強參考 參考計數為 1
// 所以 ARC 會保留住這個實體使用的記憶體

// 接著再指派給另外兩個變數
reference2 = reference1
reference3 = reference1

// 這時這個實體多了 2 個強參考 總共為 3 個強參考
// 也就是目前的參考計數為 3

// 接著將其中兩個變數指派為 nil 斷開他們的強參考
reference1 = nil
reference2 = nil

// 目前還有 1 個強參考 參考計數為 1
// 所以 ARC 仍然會保留住記憶體

// 最後將第三個變數也指派為 nil 斷開強參考
reference3 = nil

// 這時這個實體已經沒有強參考了 參考計數為 0
// ARC 就會將記憶體釋放掉

```

<a name="strong_reference_cycles"></a>
### 類別實體間的強參考循環

ARC 在大部分時間都可以運作順利，但在有些情況下會造成強參考永遠不會歸零，進而發生記憶體洩漏(`memory leak`)的問題。

以下是一個例子，兩個類別彼此都擁有對方強參考的屬性，一個實體要釋放記憶體前，必須先釋放對方強參考，而對方要釋放前也是要原本實體先釋放，進而產生**強參考循環**：

```swift
// 定義一個類別 Person
// 有一個屬性為可選 Apartment 型別 因為人不一定住在公寓內
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
}

// 定義一個類別 Apartment
// 有一個屬性為可選 Person 型別 因為公寓不一定有住戶
class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    var tenant: Person?
}

// 宣告一個變數為可選 Person 型別 並生成一個實體
var joe: Person? = Person(name: "Joe")
// 生成實體後 其內的 apartment 屬性沒有指派值 初始化為 nil
// 目前這個實體的強參考有 1 個 參考計數為 1

// 宣告一個變數為可選 Apartment 型別 並生成一個實體
var oneUnit: Apartment? = Apartment(unit: "5A")
// 生成實體後 其內的 tenant 屬性沒有指派值 初始化為 nil
// 目前這個實體的強參考有 1 個 參考計數為 1

```

接著把這兩個不同的實體聯繫起來，如下：

```swift
joe!.apartment = oneUnit
oneUnit!.tenant = joe
// 這時這兩個實體 各別都有 2 個強參考

//如果此時將 2 個變數斷開強參考
joe = nil
oneUnit = nil

// 這時這 2 個實體 各別仍還是有 1 個指向對方的強參考
// 也就造成記憶體無法釋放

```

##### Hint

- 前面章節提過，上述程式中，在實體後的驚嘆號(`!`)指的是將一個可選型別強制解析，也就是[隱式解析可選型別](../ch1/types.md#implicitly_unwrapped_optional)。

<a name="resolving_strong_reference_cycles"></a>
### 解決實體間的強參考循環

Swift 提供了兩種辦法來解決強參考循環，分別是弱參考(`weak reference`)及無主參考(`unowned reference`)。

這兩種參考也能參考實體，但因為不是強參考，所以不會保留住實體的參考(也就是這個實體的參考計數不會增加)。

而兩者的差別在於，如果一個**參考這個實體的變數**在生命週期中，可能會為`nil`時，就使用弱參考，而在初始化之後不會再變為`nil`的則是使用無主參考。

#### 弱參考

弱參考(`weak reference`)也能參考實體，但不會保留住參考的實體(所以這個實體的參考計數不會增加)。而一個**參考這個實體的變數**在生命週期中可能沒有值(為`nil`)時，就使用弱參考。

弱參考必須宣告為**變數**，表示可以被修改，同時也必須是**可選型別**(`optional`)，因為可能沒有值(為`nil`)。

弱參考使用`weak`關鍵字來定義，以下將前面強參考循環的例子改寫，將類別`Apartment`內的屬性`tenant`改為弱參考(因為公寓可能有時沒有住戶，即有時會沒有值，適合使用弱參考)：

```swift
class AnotherPerson {
    let name: String
    init(name: String) { self.name = name }
    var apartment: AnotherApartment?
}

class AnotherApartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    
    // 將這個屬性定義為弱參考 使用 weak 關鍵字
    weak var tenant: AnotherPerson?
}

var joe2: AnotherPerson? = AnotherPerson(name: "Joe")
var oneUnit2: AnotherApartment? = AnotherApartment(unit: "5A")
joe2!.apartment = oneUnit2

// 因為是弱參考
// 所以這個指派為實體的屬性 不會增加 joe2 參考的實體的參考計數
oneUnit2!.tenant = joe2

// 當斷開這個變數的強參考時 目前該實體的參考計數會減為 0
// 所以會將這個實體釋放
// 而所有指向這個實體的弱參考 都會被設為 nil
joe2 = nil

// 隨著上面的 joe2 被釋放
// 目前 oneUnit2 參考的實體的參考計數減為 1
// 以下再將原本的強參考斷開 參考計數減為 0 則也會將此實體釋放
oneUnit2 = nil

```

#### 無主參考

與弱參考一樣，無主參考(`unowned reference`)不會保留住參考的實體(所以這個實體的參考計數不會增加)，但不同的是，無主參考會被視為永遠有值，所以需要被定義為**非可選型別**，而因此可以直接存取，不需要強制解析(即加上驚嘆號`!`)。

無主參考使用`unowned`關鍵字來定義，以下例子將介紹一個使用者類別`Customer`與信用卡類別`CreditCard`之間的關係，使用者不一定有信用卡，但當產生出信用卡時，這信用卡一定屬於某個使用者：

```swift
// 定義一個類別 Customer
class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) {
        self.name = name
    }
}

// 定義一個類別 CreditCard
class CreditCard {
    let number: Int
    
    // 定義一個無主參考 非可選型別 因為一定會有使用者(一定有值)
    unowned let customer: Customer

    init(number: Int, customer: Customer) {
        self.number = number
        self.customer = customer
    }
}

// 宣告一個可選 Customer 的變數
var jess: Customer? = Customer(name: "Jess")

// 接著生成一個 CreditCard 實體並指派給 jess 的 card 屬性
jess!.card = CreditCard(number: 123456789, customer: jess!)
// 這個 CreditCard 實體的 customer 屬性 則使用無主參考指向 jess

// 現在 jess 指向的實體 參考計數為 1 (即 jess 這個變數強參考指向的)
// jess 內的屬性 card 指向的實體 參考計數也為 1
// (即這個 card 屬性強參考指向的)

// 而 CreditCard 實體的 customer 屬性 因為是無主參考指向 jess
// 所以不會增加參考計數

// 這時將 jess 指向的實體強參考斷開
jess = nil
// 這時這個實體的參考計數為 0 則實體會被釋放
// 指向 CreditCard 實體的強參考也會隨之斷開
// 因此也就被釋放了

```

#### 無主參考和隱式解析可選型別

除了上述兩種情況，還有一種情況為，兩個互相參考實體的屬性都必須有值，且初始化後永遠不為`nil`，這時要在其中一個類別使用**無主參考**，另一個類別使用**隱式解析可選型別**。

當初始化完成後，這兩個屬性都能被直接存取(不需要強制解析，即不用加上驚嘆號`!`)，且也避免了強參考循環。

以下的例子分別定義了類別`Country`及類別`City`，每個類別都有一個儲存對方類別實體的屬性，也就是每個國家(`Country`)都有一個首都(`capitalCity`)，而一個城市(`City`)必須屬於一個國家(`country`)：

```swift
class Country {
    let name: String

    // 定義為 隱式解析可選型別
    var capitalCity: City!

    init(name: String, capitalName: String) {
        self.name = name
        self.capitalCity=City(name:capitalName,country:self)
    }
}

class City {
    let name: String

    // 定義為 無主參考
    unowned let country: Country

    init(name: String, country: Country) {
        self.name = name
        self.country = country
    }
}

var country = Country(name: "Japan", capitalName: "Tokyo")

```

上述程式中，為了建立兩個類別的依賴關係，`City`的建構函式有一個`Country`實體的參數，並儲存為`country`屬性。`Country`的建構函式則呼叫了`City`的建構函式。

以`Country`來說：

1. 在建構器中要使用`self`代表自己本身，必須要在自己初始化完成後才行。而`Country`的建構函式中，在`name`設置完值後就完成了初始化，所以可以將`self`(即本身)當做參數傳給`City`的建構函式。
2. 而因為`Country`將屬性`capitalCity`定義為**隱式解析可選型別**，所以不需要使用可選綁定(`optional binding`)或強制解析(即在後面加驚嘆號`!`)去參考`City`，可以直接存取。

以`City`來說：

1. `City`在`country`屬性使用了無主參考，因為一個城市一定屬於一個國家，所以一定有值，且不會增加`Country`實體的參考計數。(其實就如同前面例子`Customer`之於`CreditCard`的關係)
   
以上的意義在於可以通過一條語句同時生成`Country`跟`City`的實體，而不會產生強參考循環，並且`capitalCity`屬性可以被直接存取，不需要被強制解析(即加上驚嘆號`!`)。

<a name="strong_reference_cycles_for_closures"></a>
### 閉包的強參考循環

除了前面提到的類別實體之間可能產生強參考循環，當將一個閉包(`closure`，也就是匿名函式)設置給一個類別實體的屬性時，這個閉包函式內存取了這個實體的某個屬性，或是呼叫了實體的一個方法，都會導致閉包捕獲(`capture`)了`self`，進而產生了強參考循環。

##### Hint

- 閉包所捕獲的參考會被自動視為強參考。

這個強參考循環的產生，是因為閉包也是參考型別，當把閉包設置給一個屬性時，實際上是設置了閉包的參考。

以下是一個閉包與類別實體的強參考循環的例子：

```swift
// 定義一個代表 HTML 元素的類別 HTMLElement
class HTMLElement {
    let name: String
    let text: String?

    // 定義為 lazy 屬性 表示只有當初始化完成以及 self 確實存在後
    // 才能存取這個屬性
    lazy var asHTML: Void -> String = {
        if let text = self.text {
            return "<\(self.name)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }

    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }

}

// 宣告為可選 HTMLElement 型別 以便後面設為 nil
var paragraph: HTMLElement?
      = HTMLElement(name: "p", text: "Hello, world")

// 初始化完成後 就可以存取這個屬性
print(paragraph!.asHTML())

// 這時 paragraph 指向的實體的參考計數為 2
// 一個是自己 一個是閉包

// 而這個實體也有一個強參考指向閉包

// 這時將變數指向的強參考斷開 參考計數減為 1
// 參考仍然不會被釋放 造成強參考循環
paragraph = nil

```

##### Hint

- 閉包中雖然多次使用了`self`，但只捕獲了 1 個強參考(也就是參考計數只算 1 次)

<a name="resolving_strong_reference_cycles_for_closures"></a>
### 解決閉包的強參考循環

在定義閉包時，同時定義**捕獲列表**(`capture list`)作為閉包的一部分，通過這種方式可以解決閉包和類別實體之間的強參考循環。捕獲列表中必須定義每個閉包中捕獲的參考為弱參考或無主參考(依其相互關係來決定)。

#### 定義捕獲列表

**捕獲列表**(`capture list`)中每一項都以`weak`或`unowned`關鍵字與類別實體的參考(如`self`)或初始化過的變數(如`delegate = self.delegate!`)成對組成，每一項以逗號`,`隔開，並寫在中括號`[]`內。

以下為捕獲列表的格式：

```swift
// 如果閉包有參數及返回型別 則將捕獲列表寫在他們前面
lazy var someClosure: (Int, String) -> String = {
    [unowned self, weak delegate = self.delegate!] 
      (index: Int, stringToProcess: String) -> String in
    // 閉包內執行的程式
}

// 或是省略閉包定義的參數或返回型別 讓他們可以通過上下文自動推斷
// 這時將捕獲列表放在關鍵字 in 的前面
lazy var someClosure: Void -> String = {
    [unowned self, weak delegate = self.delegate!] in
    // 閉包內執行的程式
}

```

以下則是將前面的例子`HTMLElement`中的閉包加上捕獲列表，便可以避免強參考循環：

```swift
class NewHTMLElement {
    let name: String
    let text: String?

    lazy var asHTML: Void -> String = {
        // 這邊使用無主參考 unowned
        [unowned self] in
        if let text = self.text {
            return "<\(self.name)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }

    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }

}

```


### 範例

本節範例程式碼放在 [ch2/arc.playground](https://github.com/itisjoe/swiftgo_files/tree/master/ch2/arc.playground)

