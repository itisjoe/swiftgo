# 繼承

- [基礎類別](#base_class)
- [生成子類別](#subclassing)
- [覆寫](#overriding)
- [防止覆寫](#preventing_overrides)

繼承(`inherit`)這個概念主要是類別(`class`)在使用，是物件導向的一個重要特性。一個類別可以繼承另一個類別的屬性(`property`)、方法(`method`)及其他特性。

一個類別繼承其他類別時，這個類別會被稱為**子類別**(`subclass`)。被繼承的類別則是稱為**父類別**(`superclass`，直翻會是超類別，但還是以父類別為主，以與子類別相對應)。

<a name="base_class"></a>
### 基礎類別

基礎類別(`base class`)就是不繼承於其它類別的類別。

Swift 中沒有一個通用的基礎類別，只要一個類別沒有繼承於其他類別，這個類別即為一個基礎類別。

以下先定義一個基礎類別，其實就是一個普通的類別：

```swift
// 定義一個遊戲角色職業通用的類別
class GameCharacter {
    // 攻擊速度
    var attackSpeed = 1.5
    
    // 這個職業的敘述
    var description: String {
        return "職業敘述"
    }
    
    // 執行攻擊的動作
    func attack() {
        // 無任何動作 有待子類別實作
    }
}

// 生成一個類別 GameCharacter 的實體
let oneChar = GameCharacter()

// 印出：職業敘述
print(oneChar.description)

```

上述程式為一個基礎的遊戲角色職業類別，僅是定義了一些通用的內容，接著需要再定義子類別來完備。

<a name="subclassing"></a>
### 生成子類別

生成子類別(`subclassing`)指的是基於一個基礎類別來定義一個新的類別，子類別會繼承父類別所有的特性，且還可以增加新的特性。

使用方式為在類別名稱後面加上冒號`:`，接著寫上父類別名稱，如下：

```swift
class 子類別: 父類別 {
    子類別的定義內容
}

```

以下是個例子，這邊定義一個繼承自類別`GameCharacter`的新類別`Archer`：

```swift
class Archer: GameCharacter {
    // 新增一個屬性 攻擊範圍
    var attackRange = 2.5
}

// 父類別所有的特性都一併繼承下來
let oneArcher = Archer()
// 可以直接以點語法來存取或設置一個父類別中定義過的屬性
oneArcher.attackSpeed = 1.8

```

一個子類別仍然可以再被其他類別繼承，以下再定義一個類別`Hunter`，繼承自類別`Archer`：

```swift
class Hunter: Archer {
    // 新增一個方法 必殺技攻擊
    func fatalBlow() {
        print("施放必殺技攻擊！")
    }
}

let oneHunter = Hunter()
// 這個類別一樣可以使用 Archer 及 GameCharacter 定義過的屬性及方法
print("攻擊速度為 \(oneHunter.attackSpeed), 攻擊範圍為 \(oneHunter.attackRange)")
// 當然自己新增的方法也可以使用
oneHunter.fatalBlow()

```

<a name="overriding"></a>
### 覆寫

類別繼承的同時，子類別可以重新定義父類別中定義過的特性，如實體方法(`instance method`)、型別方法(`type method`)、實體屬性(`instance property`)、型別屬性(`type property`)或下標(`subscript`)，這種行為即是覆寫(`overriding`)。

使用關鍵字`override`來表示你要覆寫這個特性(即方法、屬性或下標)。

#### 覆寫方法

以下為一個覆寫方法的例子：

```swift
// 使用並改寫前面定義的類別 Hunter 
class Hunter: Archer {
    // 覆寫父類別的實體方法
    override func attack() {
        print("攻擊！這是獵人的攻擊！")
    }

    // 省略其他內容
}

let oneHunter = Hunter()
oneHunter.attack()
// 即會印出覆寫後的內容：攻擊！這是獵人的攻擊！

```

#### 覆寫屬性

覆寫屬性時，需要使用`getter`(以及有時可省略的`setter`)來覆寫繼承來的屬性，且一定要寫上屬性的**名稱**及**型別**，這樣才能確定是從哪一個屬性繼承而來的。

##### Hint

- 可以將一個繼承來的唯讀屬性覆寫為一個讀寫屬性，但不行將一個讀寫屬性覆寫為唯獨屬性。即原本有`setter`的話，覆寫時就一定要有`setter`。

以下是一個例子：

```swift
// 使用並改寫前面定義的類別 Hunter
class Hunter: Archer {
    // 覆寫父類別的屬性 重新實作 getter 跟 setter
    override var attackSpeed: Double {
        get {
            return 2.4
        }
        set {
            print(newValue)
        }
    }

    // 省略其他內容
}

```

#### 覆寫屬性觀察器

覆寫屬性時，通常可以加上屬性觀察器(`property observer`)，但以下兩點要注意：

- 當繼承的屬性為**常數儲存型屬性**或**唯讀計算型屬性**時，不能加上屬性觀察器，因為這兩者的屬性無法再被設置，所以`willSet`跟`didSet`對它們沒有意義。
- 覆寫時不能同時有`setter`跟屬性觀察器(`willSet`跟`didSet`)，因為`setter`中即可做到屬性觀察器的功能要求。

雖然說是覆寫，但如果覆寫的父類別屬性也有屬性觀察器，其實子類別跟父類別兩者的屬性觀察器都會被執行，例子如下：

```swift
// 使用並改寫前面定義的類別 Archer
class Archer: GameCharacter {
    // 覆寫一個屬性 重新實作 getter 跟 setter
    override var attackSpeed: Double {
        willSet {
            print("Archer willSet")
        }
        didSet {
            print("Archer didSet")
        }
    }

    // 省略其他內容
}

// 使用並改寫前面定義的類別 Hunter
class Hunter: Archer {
    // 覆寫一個屬性 重新實作 getter 跟 setter
    override var attackSpeed: Double {
        willSet {
            print("Hunter willSet")
        }
        didSet {
            print("Hunter didSet")
        }
    }
    
    // 省略其他內容
}

let oneHunter = Hunter()
// 設置新的值 會觸發 willSet 跟 didSet
oneHunter.attackSpeed = 1.8
// 依序會印出：
// Hunter willSet
// Archer willSet
// Archer didSet
// Hunter didSet

```

上述程式中可以知道，`willSet`觸發時，會先執行子類別的再來才是父類別的，而`didSet`則是相反，先執行父類別的再來才是子類別的。

#### 存取父類別的屬性、方法及下標

在前面章節介紹類別的時候，提到類別有一個隱藏的內建屬性`self`，可以在方法中代表類別本身。而當繼承自另一個類別時，可以使用`super`屬性來存取父類別的屬性、方法或下標。

- 方法`someMethod()`的覆寫實作裡，使用`super.someMethod()`來呼叫父類別的`someMethod()`方法。
- 屬性`someProperty`的`getter`及`setter`覆寫實作裡，使用`super.someProperty`來存取父類別的`someProperty`屬性。
- 下標的覆寫實作裡，使用`super[someIndex]`來存取父類別的下標。

以下為一個例子：

```swift
// 使用並改寫前面定義的類別 Hunter
class Hunter: Archer {
    // 覆寫一個屬性 並使用父類別原先的屬性值
    override var description: String {
        return super.description + " 精通箭術的獵人"
    }
    
    // 省略其他內容
}

let oneHunter = Hunter()

// 印出：職業敘述 精通箭術的獵人
print(oneHunter.description)

```

<a name="preventing_overrides"></a>
### 防止覆寫

可以在類別的方法、屬性或下標前面加上`final`，來防止它們被覆寫，使用方式為`final var`、`final func`或是`final class func`這樣。

甚至也可以在整個類別的關鍵字`class`前面加上`final`，這樣整個類別都不能再被繼承。

