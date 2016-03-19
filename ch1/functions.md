# 函式

函式是一個獨立的程式碼區塊，用來完成特定任務。函式命名的方式與常數變數一樣，但名稱後面需要加上小括號`()`，像是前面章節很常使用的`print()`，就是一個函式。建立一個函式要使用`func`語句，函式格式如下：

```swift
func 函式名稱(參數: 參數型別) -> 返回值型別 {
    內部執行的程式
    return 返回值
}

```

最簡單的函式格式如下：

```swift
func 函式名稱() {
    內部執行的程式
}

```

沒有參數也沒有返回值，以下是一個例子：

```swift
// 建立一個函式
func simpleOne() {
    print("It is a simple function .")
}

// 呼叫函式
simpleOne()
// 這樣就會執行函式內的程式 這邊是一個簡單的功能 印出 It is a simple function .

```

### 函式參數

函式可以**傳入參數**，參數需要明確標註型別，會將參數指派給一個常數，格式如下：

```swift
func 函式名稱(參數指派給的常數: 型別標註) {
    獨立的程式碼區塊
}

```

以下是一個例子：

```swift
// 建立函式 有一個型別為 Int 的參數
// 函式的功能是將帶入的參數加一 並印出來
func addOne(number: Int) {
    // number 即為被指派參數的常數 只能在函式內部範圍內使用
    print(number + 1)
}

// 呼叫函式 傳入整數 12
addOne(12)
// 印出 13

```

#### 多重參數函式

函式如果有超過一個參數時，要依序將參數填入，並以逗號`,`隔開，以下是一個例子：

```swift
// 建立一個有兩個參數的函式 參數的型別分別為 String 及 Int
func hello(name: String, age: Int) {
    print("\(name) is \(age) years old .")
}

// 呼叫函式
hello("Jack", age: 25)

```

上述程式中可以看到呼叫函式時，第二個參數前需要標註其對應的名稱。

##### Hint：如果函式有更多參數，除了第一個參數不用之外，第二個以及之後的參數都需要標註其對應的名稱。

#### 函式參數名稱

前面提到呼叫函式時，第二個以及之後的參數都需要標註其對應的名稱，其實可以為函式特別設定這個名稱，稱作**外部參數名稱**(external parameter name)，而原先設定的名稱為**內部參數名稱**(local parameter name)。

- **外部參數名稱**用於呼叫函式時，標註給函式的參數。
- **內部參數名稱**用於函式內部操作。

以下為一個有外部參數名稱及內部參數名稱的函式格式：

```swift
func 函式名稱(外部參數名稱1 內部參數名稱1: 型別1, 外部參數名稱2 內部參數名稱2: 型別2) {
    // 函式內部執行的程式
}

```

以下為一個例子：

```swift
func hello(name n: String, age a: Int) {
    // n 跟 a 為內部參數名稱 在函式內部使用
    print("\(n) is \(a) years old .")
}

// name 跟 age 為外部參數名稱 呼叫函式時要標註在參數之前
hello(name: "Jack", age: 25)

```

前面提到第一個參數前可以不用寫外部參數名稱，如果其後的參數也不想寫外部參數名稱時，只要使用下底線`_`即可，以下為一個例子：

```swift
// 第二個參數的外部參數名稱 使用下底線 _ 替換
func hello(name: String, _ age: Int) {
    print("\(name) is \(age) years old .")
}

// 呼叫函式時 第二個參數就可以不用寫外部參數名稱
hello("Jack", 33)

```

Swift 中，函式第一個參數是預設不需要加上外部參數名稱，呼叫函式時也不需要寫。而其後的參數，可以使用其內部參數名稱作為外部參數名稱，就如同前面一開始介紹多重參數函式時的方式，如下：

```swift
// 第一個參數不需要外部參數名稱
// 第二個及之後的參數 可以使用其內部參數名稱作為外部參數名稱 這邊就是 age
func hello(name: String, age: Int) {
    print("\(name) is \(age) years old .")
}

// 呼叫函式時 第二個參數的外部參數名稱是 age 而內部參數名稱也是 age
hello("Jack", age: 25)

```

Swift 會這樣稍微複雜的設計函式參數名稱，是為了讓呼叫函式時，語意可以更明顯，以下是一個例子：

```swift
func sayHello(to name: String, and name2: String) {
    print("Hello \(name) and \(name2) !")
}
// 外部參數名稱設為 to 跟 and
// 這行看起來就像個完整的英文句子 可以明顯地知道這個函式要幹嘛
sayHello(to: "Joe", and: "Amy")

```

#### 預設參數值

可以對函式的參數設定一個預設值，如果未傳入值時，則內部就使用這個預設值。如下：

```swift
func someFunction(number: Int = 12) {
    print(number)
}
someFunction(6) // 印出 6
someFunction() // 沒有傳入值 則會使用預設值 印出 12

```

##### Hint：有多重參數時，最好將有預設值的參數放在參數列表的最後，這樣呼叫函式時，可以保證其他無預設值參數的順序是一致的。

#### 可變參數

函式的可變參數(variadic parameter)可以接受零個或多個值，呼叫函式時，使用可變參數來傳入數量不確定的參數，使用方法是在參數的型別後面加上三個點(`...`)，以下是個例子：

這個函式有一個型別為`Double...`的可變參數，在函數內會轉成一個型別為`[Double]`的陣列常數。

```swift
func arithmeticMean(numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5) // 返回 3.0
arithmeticMean(3, 8, 19) // 返回 10.0

```

##### Hint

- 一個函式只能有一個可變參數
- 函式如果有預設值參數，也有可變參數時，必須把可變參數放在參數列表的最後。


#### 常數參數和變數參數

函數參數傳入後預設為常數，如果在函式內改變其值會發生錯誤，所以在需要的時候，可以將參數設為變數，使用方法為在參數名稱前加上`var`，以下是個例子：

```swift
// 將傳入參數設為變數 在參數前加上 var
func newNumber(var number: Int) -> Int {
    if number < 10 {
        // 所以這邊可以修改變數
        // 當 number 小於 10 時 乘以 10
        number *= 10
    }

    return number
}

newNumber(8) // 印出 80
newNumber(20) // 印出 20

```

##### Hint：這個變數參數只會在函式內部存在，外部無法使用。


#### 輸入輸出參數

前面剛說過的變數參數只能在函式內部中使用，如果想要一個可以修改參數的函式，並在呼叫函式結束後，這個修改仍然存在，則必須將參數設定為**輸入輸出參數**(In-Out Parameters)。使用方式為：

- 建立函式時，在參數名稱前加上`inout`。此參數不能有預設值，也不能再設為可變參數，也不能再加上`var`或`let`。
- 當呼叫函式，傳入的參數作為輸入輸出參數時，需要在參數前加上`&`。這個參數只能是一個**變數**，不能是常數、字面量(單純的數值或字串)。

以下是個例子：

```swift
func newNumber(inout number: Int) {
    number *= 2
}

var n = 10
print(n) // 這時 n 為 10

// 傳入的參數在函式結束後 改變仍然存在
newNumber(&n)

print(n) // 所以這時再印出 就會是 20

```


### 函式返回值

函式除了可以傳入參數，同時也**可以返回值**，使用方式是在函式`()`後面接著一個**減號跟大於**`->`，然後再接著寫返回值的型別標註，函式內部要使用`return`語句後接著返回值來返回。使用`return`後會隨即終止函式的動作，函式內部其後的程式都不會繼續執行。以下是個例子：

```swift
// 建立函式 有一個型別為 Int 的參數以及一個型別為 Int 的返回值
// 函式的功能是將帶入的參數加十 並返回
func addTen(number: Int) -> Int {
    let n = number + 10
    // 使用 return 來返回值 這個返回值的型別要與上面標註的相同
    return n
}

// 呼叫函式 傳入整數 12 會返回 22
let newNumber = addTen(12)
print(newNumber) // 印出 22

```


#### 多重返回值函式

函式可以有不只一個的返回值，返回值超過一個時以一個元組(`Tuple`)返回，元組內包含著要回傳的每個值的型別標註。在建立函式時也可以給返回元組的每個值加上名稱。以下是一個例子：

```swift
// 建立函式 有一個型別為 Int 的參數, 返回兩個型別為 Int 的值
func findNumbers(number: Int) -> (Int, Int) {
    let n = number + 10
    // 返回一個元組
    return (number, n)
    
    // 合併成一行直接返回也是可以
    // return (number, number + 10)
}

// 呼叫函式 傳入整數 12 會返回 (12, 22)
let numbers = findNumbers(12)
// numbers 為一個元組 可以自 0 開始算 依序取得其內的值
print("\(numbers.0) and \(numbers.1)") // 印出 12 and 22


// 建立另一個函式 將上面函式中返回的元組內的值加上名稱
func findNumbers2(number: Int) -> (oldNumber: Int, newNumber: Int) {
    let n = number + 10
    return (number, n)
}

// 呼叫函式 傳入整數 24 會返回 (24, 34)
let numbers2 = findNumbers2(24)
// 這邊即可使用建立函式時 返回元組內的值設定的名稱
print("\(numbers2.oldNumber) and \(numbers2.newNumber)") // 印出 24 and 34

```


#### 可選元組返回型別

如果整個返回的元組可能會**沒有值**(`nil`)，可以將此返回元組設為可選元組型別，使用方式為在括號`()`後面接著一個問號`?`，像是`(Int, String)?`或是`(String, Int, Int)?`。以下是個例子：

```swift
// 建立函式 參數為一個型別為 [Int] 的陣列
// 返回值為 包含兩個型別為 Int 的元組 或是 nil
func findNumbers(arr: [Int]) -> (Int, Int)? {
    // 檢查傳入的陣列 如果其內沒有值的話 就直接返回 nil
    if arr.isEmpty {
        return nil
    }
    let n = arr[0] + 10
    let n2 = arr[0] + 100
    return (n, n2)
}

// 呼叫函式
// 因為返回的是一個可選型別 這邊先做可選綁定的動作 確定有值後再印出來
if let numbers = findNumbers([11,22,33]) {
    print("\(numbers.0) and \(numbers.1)")
}
// 如果傳入的陣列 內部沒有值
if let numbers = findNumbers([]) {
    print("這裡不會被印出來")
}

```


### 函式型別

函式也是一種型別，由函式的參數型別和返回值型別組成。

```swift
func someFunction(a: String, b: Int) -> Int {
    // 函式內部程式
}

```

上述程式中的函式，這個函式型別就標註為`(String, Int) -> Int`，意思就是有兩個型別依序為`String`跟`Int`的參數，會返回一個型別為`Int`的值。

以下是另一個例子，如果沒有參數也沒有返回值時，函式型別標註為`() -> Void`。

```swift
func hello() {
    print("Hello !")
}

```

這個函式型別也可以標註為`() -> ()`。實際上，沒有返回值的函式，會返回一個特殊的值，叫做`Void`，也就是一個空的元組(tuple)，沒有任何元素，可以寫成`()`。


### 使用函式型別

前面剛提過函式是一種型別，所以也可以將變數或常數宣告為一個函式，然後可以指派一個適當的函式。

以下是一個例子，宣告一個變數`mathFunction`，是一個型別為`(Int, Int) -> Int`的函式，最後指派一個已經存在且型別相同的函式：

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts

```


#### 函式型別作為參數型別

函式可以作為另一個函式的參數，以下是個例子：

```swift
// 建立一個將兩個整數相加的函式
func addTwoInts(number1: Int, number2: Int) -> Int {
    return number1 + number2
}

// 建立另一個函式，有三個參數依序為
// 型別為 (Int, Int) -> Int 的函式, Int, Int
func printMathResult(mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}

// 將一個函式(addTwoInts)傳入函式(printMathResult)
printMathResult(addTwoInts, 3, 5)

```

上述程式中，作為參數的函式只需要型別正確，不用了解其內的程式如何運作，這表示可以將`printMathResult`函式一部分的操作交給呼叫函式的人來實作，也是以一種型別安全(`type-safe`)的方式來保證傳入函式的呼叫是正確的。


#### 函式型別作為返回型別

函式可以作為另一個函式的返回值，使用方式為在返回箭頭(`->`)後寫一個完整的函式型別。以下是個例子：

```swift
// 建立一個將傳入的參數加一的函式
func stepForward(input: Int) -> Int {
    return input + 1
}

// 建立一個將傳入的參數減一的函式
func stepBackward(input: Int) -> Int {
    return input - 1
}

// 建立一個參數為布林值的函式 會返回一個函式
// 根據布林值返回上述兩個函式的其中一個
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    return backwards ? stepBackward : stepForward
}

// 宣告一個整數常數
let number = 3
// 宣告一個函式常數
let someFunction = chooseStepFunction(number > 0)
// 根據 chooseStepFunction 函式的內容 傳入 true 時 會返回 stepBackward 函式
// 所以 someFunction 會被指派為 stepBackward

someFunction(10) // 返回 9

```

##### Hint：這邊宣告函式常數時沒有標註型別，因為就如同其他變數或常數宣告時，Swift 會根據指派的函式自動判斷出型別。


### 嵌套函式 Nested Functions

到目前為止所使用的函式都叫全域函式(global functions)，定義在全區域中，每個地方都可以使用。

如果將函式建立在另一個函式中，稱作嵌套函式(nested functions)，被建立在其內的函式只能在裡面使用，也可以當做返回值返回以讓其他地方也可以使用，以下為一個例子：

```swift
// 改寫前面的內容 將兩個函式建立在這個函式內
// 同樣是依據傳入的布林值 返回不同的函式
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int {
        return input + 1
    }

    func stepBackward(input: Int) -> Int {
        return input - 1
    }

    return backwards ? stepBackward : stepForward
}

let number = -35
let someFunction = chooseStepFunction(number > 0)

someFunction(10) // 返回 11

```

