# 型別轉換

Swift 提供兩個運算子：`is`跟`as`來檢查一個值的型別以及將一個值轉換成另一種型別。

基於類別繼承的特性，一個子類別繼承於另一個父類別時，這個子類別的型別除了可以是自己之外，也可以是父類別型別。下面例子以三個類別(一個基礎類別及兩個繼承自它的子類別)示範`is`及`as`的使用方法：

```swift
// 首先定義一個類別 GameCharacter
class GameCharacter {
    var name: String
    init(name: String) {
        self.name = name
    }
}

// 接著定義兩個繼承自 GameCharacter 的類別
class Archer: GameCharacter {
    var intro: String
    init(name: String, intro: String) {
        self.intro = intro
        super.init(name: name)
    }
}

class Warrior: GameCharacter {
    var description: String
    init(name: String, description: String) {
        self.description = description
        super.init(name: name)
    }
}

```

根據上面建立的類別，接著宣告一個陣列常數，包含 2 個`Archer`實體及 1 個`Warrior`實體，而初始化時會自動推斷出`Archer`跟`Warrior`有一個共同的父類別`GameCharacter`，所以這個陣列會自動推斷型別為`[GameCharacter]`，如下：

```swift
// gameTeam 的型別被自動推斷為 [GameCharacter]
let gameTeam = [
    Archer(name: "one", intro: "super power"),
    Warrior(name: "two", description: "good fighter"),
    Archer(name: "three", intro: "not bad")
]

```


### 型別檢查

使用型別檢查運算子`is`來檢查一個實體是否屬於一個類別。檢查後會返回一個布林值，是這個類別會返回`true`，反之則返回`false`。
以下使用`for-in`來遍歷前面宣告的陣列`gameTeam`：

```swift
// 用來計算弓箭手跟戰士的數量
var archerCount = 0
var warriorCount = 0

for character in gameTeam {
    // 檢查是否是弓箭手或戰士
    if character is Archer {
        archerCount += 1
    } else if character is Warrior {
        warriorCount += 1
    }
}

// 最後印出：這隻隊伍有 2 個弓箭手跟 1 個戰士。
print("這隻隊伍有 \(archerCount) 個弓箭手跟 \(warriorCount) 個戰士。 ")

```

你也可以用`is`來檢查一個類別是否遵循了某個協定(`protocol`)。

後面章節會正式介紹協定。


### 向下型別轉換

一個類別的常數或變數，可能實際上是屬於一個子類別(像是前面提的例子)。當遇到這種情況，可以嘗試使用型別轉換運算子`as?`或`as!`將其向下型別轉換至子類別型別。

如果不確定向下型別轉換是否能夠成功，則使用`as?`來轉換，這會返回一個可選值，如果向下型別轉換失敗會返回一個`nil`，這樣的特性可以讓你檢查向下型別轉換是否成功。

如果確定向下型別轉換一定成功時，則可以使用強制性的`as!`來轉換。但如果轉換至一個錯誤的型別，則會觸發程式運行時錯誤。

以下仍用前面宣告的陣列`gameTeam`來做例子：

```swift
for character in gameTeam {
    if let oneChar = character as? Archer {
        print("弓箭手的名字：\(oneChar.name)，介紹：\(oneChar.intro)")
    } else if let anotherChar = character as? Warrior {
        print("戰士的名字：\(anotherChar.name)，描述：\(anotherChar.description)")
    }
}

// 使用可選綁定來檢查是否轉換成功 會依序印出：
// 弓箭手的名字：one，介紹：super power
// 戰士的名字：two，描述：good fighter
// 弓箭手的名字：three，介紹：not bad

```


### Any 及 AnyObject 的型別轉換

Swift 為不確定的型別提供了兩種特殊型別別名：

- AnyObject：可以表示任何類別型別的實體。
- Any：可以表示為任何型別。

##### Hint

- 為了型別安全，我們應該明確地指定值或實體的型別，除非是真的必要或確切需要才使用`Any`跟`AnyObject`。

#### AnyObject

這邊使用前面定義的類別`Archer`做例子：

```swift
let someObjects: [AnyObject] = [
    Archer(name: "one", intro: "super power"),
    Archer(name: "two", intro: "not bad")
]

// 我們明確的知道這個陣列只包含著 Archer 實體
// 所以可以使用強制性的 as! 來向下轉換至 Archer 型別
for object in someObjects {
    let oneChar = object as! Archer
    print("弓箭手的名字：\(oneChar.name)，介紹：\(oneChar.intro)")
}

// 依序會印出：
// 弓箭手的名字：one，介紹：super power
// 弓箭手的名字：two，介紹：not bad

```

以這個例子來說，可以直接將陣列強制向下轉換(`as!`)為`[Archer]`型別，這樣可以讓程式碼更為簡潔，如下：

```swift
for object in someObjects as! [Archer] {
    print("弓箭手的名字：\(object.name)，介紹：\(object.intro)")
}

```

#### Any

以下宣告一個`[Any]`型別的陣列，所以其內可以放進各種型別的值，如下：

```swift
var things = [Any]()

// 依序加入 浮點數, 字串, 元組, Archer 實體, 閉包
things.append(3.1415926)
things.append("Hello, world")
things.append((3.0, 5.0))
things.append(Archer(name: "one", intro: "super power"))
things.append({ (name: String) -> String in "Hello, \(name)" })

// 再以 for-in 遍歷陣列 使用 switch 配對每一個值
for thing in things {
    switch thing {
    case let someDouble as Double where someDouble > 0:
        print("浮點數為 \(someDouble)")
    case let someString as String:
        print("字串為 \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("元組為 \(x), \(y)")
    case let oneChar as Archer:
        print("弓箭手的名字：\(oneChar.name)，介紹：\(oneChar.intro)")
    case let stringConverter as String -> String:
        print(stringConverter("Jess"))
    default:
        print("沒有配對到的值")
    }
}

// 依序印出：
// 浮點數為 3.1415926
// 字串為 "Hello, world"
// 元組為 3.0, 5.0
// 弓箭手的名字：one，介紹：super power
// Hello, Jess

```

