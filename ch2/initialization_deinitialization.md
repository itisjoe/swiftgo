# 建構過程及解構過程

建構過程就是要生成一個類別、結構或列舉的實體時進行初始化的過程，這個過程必須為實體中每個屬性設置初始值及其他視需求需要執行的程式。

與此相對的，解構過程則是在類別實體被釋放前，執行特定的清除工作。


### 建構器

建構過程則是透過建構器(`initializer`)實作，也就是`init()`方法，最簡單的一個形式如下：

```swift
init() {
    執行的建構過程
}

```

以下是一個例子：

```swift
// 定義一個類別 SomeClass 並在建構器中指派初始值給屬性 number
class SomeClass {
    let number: Int

    init() {
        number = 12
    }
}

```


### 設置儲存屬性的初始值

在建構過程結束前，這個類別的儲存屬性都必須被指派一個明確的值，可以是定義時直接指派，或是在建構器中為屬性指派。如下：

```swift
class SomeClass {
    let number: Int = 20
    let anotherNumber: Int

    init() {
        anotherNumber = 12
    }
}

```

上述程式可以看到，有一個屬性是定義時即指派值，另一個屬性則是在建構器中被指派。

#### 可選型別的屬性

如果一個儲存屬性依照需求或規劃，在建構過程中沒有辦法被指派或是需要在之後可以被設置為`nil`，可以將其定義為可選型別(`optional type`)，這樣它會被初始化為`nil`，則建構過程中可以不用被指派值，例子如下：

```swift
class OneQuestion {
    var question: String
    
    // 可選型別 即可在建構過程時不用指派值
    var answer: String?

    init() {
        // 僅指派一個型別為 String 的屬性
        question = "問題的題目"
    }
}

let someQuestion = OneQuestion()
// 這時才將 anser 指派值
someQuestion.answer = "答案隨後跟上"

```

#### 使用閉包或函式設置屬性的預設值

可以使用閉包或全域函式來為屬性提供預設值，以下是一個閉包的例子：

```swift
class SomeClass {
    let numbers: [Int] = {
        var temporaryNumbers = [Int]()
        var isBlack = false
        for i in 1...10 {
            temporaryNumbers.append(i)
        }
        return temporaryNumbers
    }()
}

```

上述程式可以看到閉包後緊接著一對小括號`()`，表示要立即執行這個閉包並返回閉包的值。


### 為建構器提供參數

建構器可以加入參數，使用方式與函式跟方法類似，例子如下：

```swift
struct SimpleMath {
    var number: Double
    init(huge n: Double) {
        number = n * 100
    }
    init(tiny n: Double) {
        number = n / 10
    }
}

let oneSimpleMath = SimpleMath(huge: 30.0)
print(oneSimpleMath.number) // 印出 3000.0

let anotherSimpleMath = SimpleMath(tiny: 10.0)
print(anotherSimpleMath.number) // 印出 1.0

```

上述程式為一個結構，可以看到建構器可以不只有一個，由參數的不同來辨別要使用哪一個建構器。

與函式跟方法不一樣的是，因為建構器都叫做`init()`，為了辨識，所以不管有幾個參數，外部參數名稱預設都是必須的(函式跟方法的第一個參數預設是省略外部參數名稱)。

#### 內部參數名稱及外部參數名稱

與函式跟方法一樣，可以使用其內部參數名稱作為外部參數名稱，如下：

```swift
// 定義一個結構 有兩個建構器
struct Color {
    let red, green, blue: Double
    
    // 這個建構器有寫外部參數名稱跟內部參數名稱
    init(red r: Double, green g : Double, blue b: Double) {
        self.red   = r
        self.green = g
        self.blue  = b
    }
    
    // 這個建構器則是合併成一個參數名稱 外部跟內部參數名稱相同
    init(white: Double) {
        red   = white
        green = white
        blue  = white
    }
}

var oneColor = Color(red: 0.9, green: 0.5, blue: 0.5)
var anotherColor = Color(white: 1.0)

```

#### 省略外部參數名稱

與函式跟方法一樣，可以省略其外部參數名稱，使用下底線`_`替代外部參數名稱，如下：

```swift
struct SomeNumbers {
    let number: Int
    // 使用下底線 _ 表示要省略外部參數名稱
    init(_ n: Int) {
        number = n
    }
}

// 生成一個實體時 參數前就不需要有外部參數名稱
var oneNumbers = SomeNumbers(9)

```

### 預設建構器


### 結構的成員逐一建構器


### 值型別的建構器代理


### 類別的繼承與建構過程

#### 指定建構器與便利建構器

#### 類別的建構器代理

#### 建構器的繼承與覆寫

#### 建構器的自動繼承

例子：


### 可失敗的建構器

#### 列舉型別的可失敗建構器

#### 帶原始值的列舉型別的可失敗構造器

#### 類別的可失敗建構器

#### 覆寫一個可失敗建構器

#### 可失敗建構器 init!


### 必要建構器


### 解構器









