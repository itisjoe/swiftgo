# 擴展

- [擴展語法](#extension)
- [計算屬性](#property)
- [方法](#method)
- [建構器](#initializer)
- [下標](#subscript)
- [巢狀型別](#nested_type)

擴展(`extension`)是 Swift 一個重要的特性，它可以為已存在的列舉、結構、類別和協定添加新功能，而且不需要修改該型別原本定義的程式碼。擴展也可以使用在內建的型別上，像是`Int`、`Double`或`String`等等。

Swift 的擴展可以：

- 新增計算屬性(包含實體屬性和型別屬性)。
- 定義實體方法和型別方法(不能覆寫已存在的方法)。
- 提供新的建構器。
- 定義下標。
- 定義和使用新的巢狀型別。
- 讓一個已存在的型別遵循某個協定。

<a name="extension"></a>
### 擴展語法

使用`extension`關鍵字來定義一個擴展，格式如下：

```swift
extension 某個型別 {
    新增的程式內容
}

```

當你對一個已存在的型別新增一個擴展之後，擴展的新功能可以立即給該型別的所有實體使用，即使這個實體在定義擴展前就已經生成了也是可以。

另外，擴展也可以讓一個已有的型別遵循一個或多個協定，格式就如同結構及類別一樣：

```swift
extension 某個型別: 協定, 另一個協定, 又另一個協定 {
    新增的程式內容
}

```

後面章節會正式介紹協定(`protocol`)。

<a name="property"></a>
### 計算屬性

擴展可以對內建的型別增加**計算實體屬性**與**計算型別屬性**。下面例子為內建的`Double`型別增加了 3 個計算實體屬性，用來表示常見的距離單位：

```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
}

```

定義好新增的擴展之後，就可以直接使用，使用方法就如同普通的屬性一樣使用點語法再緊接著屬性名稱，如下：

```swift
// 直接對型別 Double 的值取得屬性
let aMarathon = 42.km + 195.m

// 印出：馬拉松的距離全長為 42195.0 公尺
print("馬拉松的距離全長為 \(aMarathon) 公尺")

```

##### Hint

- 擴展不能新增儲存屬性，也不能為已有的屬性添加屬性觀察器(`property observer`)。

<a name="method"></a>
### 方法

擴展可以為已有的型別新增實體方法與型別方法。以下例子為內建的`Int`型別新增一個實體方法：

```swift
// 新增一個實體方法 有一個參數 型別為 () -> Void 的閉包
// 這個新增的實體方法會執行這個閉包
// 執行次數為：這個整數本身代表數字
extension Int {
    func repetitions(task: () -> Void) {
        for _ in 0..<self {
            task()
        }
    }
}

// 會依序印出 3 次：Hello!
// 這邊使用 尾隨閉包 簡化語法
3.repetitions {
    print("Hello!")
}

```

#### 變異實體方法

擴展也可以新增變異實體方法，與一般變異方法一樣在前面加上`mutating`關鍵字，下面例子為內建的`Int`型別新增一個變異實體方法：

```swift
// 為內建的 Int 型別新增一個變異實體方法：取得這個整數的平方數
extension Int {
    mutating func square() {
        self = self * self
    }
}

// 先宣告一個整數
var oneInt = 5

// 接著呼叫方法 這裡會得到 25
oneInt.square()

```

<a name="initializer"></a>
### 建構器

擴展能為**類別**新增**便利建構器**(`convenience initializer`)，但不能新增**指定建構器**(`designated initializer`)跟**解構器**(`deinitializer`)。

以下例子為一個**結構**新增一個建構器。在介紹結構時有提過，如果沒有為結構定義建構器時，結構會有一個自動生成的**成員逐一建構器**(`memberwise initializer`)，而這邊因為是使用擴展為結構新增建構器，所以原本的成員逐一建構器仍然可以使用：

```swift
// 定義一個結構 會有一個自動生成的成員逐一建構器
struct GameCharacter {
    var hp = 100,mp = 100, name = ""
}

// 為結構 GameCharacter 定義一個建構器的擴展
extension GameCharacter {
    init(name:String) {
        self.name = name
        print("新名字為 \(name)")
    }
}

// 使用擴展後定義的建構器
let oneChar = GameCharacter(name: "弓箭手")

// 原本的成員逐一建構器仍然可以使用
let twoChar = GameCharacter(hp: 200, mp: 50, name: "戰士")

```

##### Hint

- 使用擴展新增一個新的建構器時，仍然需要確保建構過程中的每一個實體的完全初始化。

<a name="subscript"></a>
### 下標

擴展可以為已有的型別新增下標。下面例子為內建的`Int`型別增加下標：

```swift
// 定義下標 取得一個整數從個位數算起第幾個數字
// 索引值：0 為取得個位數, 1 為取得十位數, 2為取得百位數 依此類推
extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
        for _ in 0..<digitIndex {
            decimalBase *= 10
        }
        return (self / decimalBase) % 10
    }
}

// 接著就可以得到每一個位數的數字
// 得到個位數：9
123456789[0]

// 得到千位數：6
123456789[3]

```

<a name="nested_type"></a>
### 巢狀型別

擴展可以為已有的列舉、結構和類別新增巢狀型別。以下為內建的 Int 型別內新增一個列舉的擴展：

```swift
// 為內建的 Int 型別內新增一個列舉的擴展
// 用來表示這個整數是負數、零還是正數
extension Int {
    enum Kind {
        case Negative, Zero, Positive
    }

    // 另外還新增一個計算屬性 用來返回列舉情況
    var kind: Kind {
        switch self {
        case 0:
            return .Zero
        case let x where x > 0:
            return .Positive
        default:
            return .Negative
        }
    }
}

// 依序會印出：Positive、Negative、Zero
for number in [3, -12, 0] {
    print(number.kind)
}

```

