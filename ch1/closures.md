# 閉包 Closures

閉包(`closure`)簡單來講就是一個匿名函式，同樣是一個獨立的程式區塊。像是前面章節提到的巢狀函式(`nested function`)就是一種閉包，可以在程式中被傳遞和使用。

閉包有三種表現方式：

- **函式**就是一種有名稱的閉包。
- **巢狀函式**就是一種有名稱且被包含在另一個函式中的閉包。
- **閉包表達式**就是使用簡潔語法來描述的一種沒有名稱的函式，可以在程式中被傳遞和使用。


### 閉包表達式 Closure Expressions

閉包表達式是一種利用簡潔語法建立匿名函式的方式。同時也提供了一些優化語法，可以使得程式碼變得更好懂及直覺。閉包表達式的格式如下：

```swift
{ (參數) -> 返回值型別 in
    內部執行的程式
}

```

上述程式中可以看到，與函式相同是以大括號`{}`將程式包起來，但省略了名稱，包著參數的小括號`()`擺到`{}`裡並接著箭頭`->`及返回值型別。然後使用`in`分隔內部執行的程式。

閉包表達式可以使用常數、變數和`inout`型別作為參數，但不能有預設值。也可以在參數列表的最後使用可變參數。元組也可以作為參數和返回值。

下面是一個例子，會將前面章節中，以函式當做另一個函式的參數開始進行，原始程式碼如下：

```swift
// 這是一個要當做參數的函式 功能為將兩個傳入的參數整數相加並返回
func addTwoInts(number1: Int, number2: Int) -> Int {
    return number1 + number2
}

// 建立另一個函式，有三個參數依序為
// 型別為 (Int, Int) -> Int 的函式, Int, Int
func printMathResult(mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}

```

這邊將函式`addTwoInts()`修改成一個匿名函式(即閉包)傳入，程式如下：

```swift
printMathResult({(number1: Int, number2: Int) -> Int in
   return number1 + number2
}, 12, 85) // 印出 97

/* 第一個參數為一個匿名函式(閉包) 如下
{(number1: Int, number2: Int) -> Int in
   return number1 + number2
}
*/
```

上述程式中可以看到函式`printMathResult()`依舊為三個參數，第一個參數為一個閉包，其後為兩個`Int`的參數。

接下來會使用上面這個例子，介紹幾種優化語法的方式，越後面介紹的程式語法會越簡潔，但使用上功能是一樣的。

#### 根據上下文推斷型別 Inferring Type From Context

因為**兩數相加閉包**是作為函式`printMathResult()`的參數傳入的，Swift 可以自動推斷其參數及返回值的型別(根據建立函式`printMathResult()`時的參數型別`(Int, Int) -> Int`)，因此這個閉包的參數及返回值的型別都可以省略，同時包著參數的小括號`()`及返回箭頭`->`也可以省略，修改如下：

```swift
printMathResult({number1, number2 in return number1 + number2}, 12, 85) // 印出 97

/* 第一個參數修改成如下
{number1, number2 in return number1 + number2}
*/

```


#### 單表達式閉包隱式回傳 Implicit Return From Single-Expression Clossures

單行表達式閉包可以通過隱藏`return`來隱式回傳單行表達式的結果，修改如下：

```swift
printMathResult({number1, number2 in number1 + number2}, 12, 85) // 印出 97

/* 第一個參數修改成如下
{number1, number2 in number1 + number2}
*/

```


#### 參數名稱縮寫 Shorthand Argument Names

Swift 自動為閉包提供參數名稱縮寫功能，可以直接以`$0`,`$1`,`$2`這種方式來依序呼叫閉包的參數。

如果使用了參數名稱縮寫，就可以省略在閉包參數列表中對其的定義，且對應參數名稱縮寫的型別會通過函式型別自動進行推斷，所以同時`in`也可以被省略，修改如下：

```swift
printMathResult({$0 + $1}, 12, 85) // 印出 97

/* 第一個參數修改成如下
{$0 + $1}
*/

```


#### 運算子函式 Operator Functions

實際上還有一種更簡潔的語法。Swift 的`String`型別定義了關於加號`+`的字串實作，其作為一個函式接受兩個數值，並返回這兩個數值相加的值。而這正好與最一開始的函式`addTwoInts()`相同，因此你可以簡單地傳入一個加號`+`，Swift會自動推斷出加號的字串函式實作，修改如下：

```swift
printMathResult(+, 12, 85) // 印出 97

// 第一個參數修改成： +

```

以上介紹了四種優化閉包的語法，使用上功能都與最一開始的閉包相同，所以可以依照需求以及合適性，使用不同的優化語法，來讓你的程式更簡潔與直覺，當然都使用完整寫法的閉包也是可以的。


### 尾隨閉包 Trailing Closures

如果需要將一個很長的閉包表達式作為最後一個參數傳遞給函式，可以使用尾隨閉包來增強函式的可讀性。 尾隨閉包是一個書寫在函式括號之後的閉包表達式，函式支援將其作為最後一個參數呼叫。以下是一個例子：

```swift
// 這是一個參數為閉包的函式
func someFunction(closure: () -> ()) {
    // 內部執行的程式
}
// 內部參數名稱為 closure
// 閉包的型別為 () -> () 沒有參數也沒有返回值

// 不使用尾隨閉包進行函式呼叫
someFunction({
    // 閉包內的程式
})
// 可以看到這個閉包作為參數 是放在 () 裡面

// 使用尾隨閉包進行函式呼叫
someFunction() {
  // 閉包內的程式
}
// 可以看到這個閉包作為參數 位置在 () 後空一格接著寫下去

```

如果函式只有閉包這一個參數時，甚或是可以將函式的`()`省略，修改如下：

```swift
// 使用尾隨閉包進行函式呼叫 省略函式的 ()
someFunction {
  // 閉包內的程式
}

```


### 捕獲值

閉包可以在其定義的上下文中捕獲(`capture`)常數或變數，即使定義這些常數或變數的原域已經不存在，閉包仍可以在閉包函式體內參考或修改這些值。

Swift 中，可以捕獲值的閉包的最簡單形式是巢狀函式，也就是定義在其他函式內的函式。巢狀函式可以捕獲並存取**外部函式**(把它定義在其中的函式)內所有的參數以及定義的常數與變數，即使這個巢狀函式已經回傳，導致常數或變數的作用範圍不存在，閉包仍能對這些已經捕獲的值做操作。

以下是一個例子：

```swift
// 定義一個函式 參數是一個整數 回傳是一個型別為 () -> Int 的閉包
func makeIncrementor(forIncrement amount: Int) -> () -> Int {
    // 用來儲存計數總數的變數
    var runningTotal = 0

    // 巢狀函式 簡單的將參數的數字加進計數並返回
    // runningTotal 和 amount 都被捕獲了
    func incrementer() -> Int {
        runningTotal += amount
        return runningTotal
    }

    // 返回捕獲變數參考的巢狀函式
    return incrementer
}

```

上述程式中可以看到，巢狀函式內部存取了的`runningTotal`及`amount`變數，是因為它從外部函式捕獲了這兩個變數的參考。而這個捕獲參考會讓`runningTotal`與`amount`在呼叫完`makeIncrementer`函式後不會消失，並且下次呼叫`incrementer`函式時，`runningTotal`仍會存在。

以下是呼叫這個函式的例子：

```swift
// 宣告一個常數
// 會被指配為一個每次呼叫就會將 runningTotal 加 10 的函式 incrementor
let incrementByTen = makeIncrementor(forIncrement: 10)
// 呼叫多次 可以觀察到每次返回值都是累加上去
incrementByTen() // 10
incrementByTen() // 20
incrementByTen() // 30

// 如果另外再宣告一個常數 會有屬於它自己的一個全新獨立的 runningTotal 變數參考
// 與上面的常數無關
let incrementBySix = makeIncrementor(forIncrement: 6)
incrementBySix() // 6

// 第一個常數仍然是對它自己捕獲的變數做操作
incrementByTen() // 40

```


### 閉包是參考型別

前面的例子中，`incrementByTen`和`incrementBySix`是常數，但這些常數指向的閉包仍可以增加其捕獲的變數值，這是因為函式與閉包都是參考型別。

參考型別就是無論將函式(或閉包)指派給一個常數或變數，實際上都是將常數或變數的值設置為對應這個函式(或閉包)的參考(參考其在記憶體空間內配置的位置)。

所以當你將閉包指派給了兩個不同的常數或變數，這兩個值都會指向同一個閉包(的參考)，如下：

```swift
// 指派給另一個常數
let alsoIncrementByTen = incrementByTen

// 仍然是對原本的 runningTotal 操作
alsoIncrementByTen() // 50

```

##### Hint

- 後面章節會正式介紹值型別與參考型別的不同。

### 非逃逸閉包

當一個閉包被當做參數傳入一個函式中，但是這個閉包在函式返回後才被執行(例如像是閉包被當做函式的返回值，然後接著去做別的操作)，這樣稱作閉包從函式中逃逸(`escape`)。而如果要明確表示一個當做參數的閉包不能從函式中逃逸，要在當做參數的閉包前標註`@noescape`，來表明這個閉包的生命週期只在這個函式體內。例子如下：

```swift
// 參數為一個閉包的函式 參數前面標註 @noescape
func someFunctionWithNoescapeClosure(@noescape closure: () -> Void) {
    // 而這個閉包的生命週期只在這個函式內
    closure()
}

```

而下面這個例子是說明一個逃逸的閉包：

```swift
// 宣告一個函式之外的變數 是一個陣列 陣列成員的型別為閉包 () -> Void
var completionHandlers: [() -> Void] = []

// 接著定義一個函式 參數為一個閉包 型別與上面陣列成員的型別一樣
func someFunctionWithEscapingClosure(completionHandler: () -> Void) {
    // 這個函式將閉包加入一個函式之外的陣列變數中
    completionHandlers.append(completionHandler)
}

// 如果將這個函式的參數前面加上 @noescape 的話會報錯誤 因為閉包逃逸了

```

另外還有一點，將閉包標註為`@noescape`，可以讓你在閉包中隱式的參考`self`，以下利用到前面定義的兩個示範函式做例子：

```swift
// 定義一個類別
class SomeClass {
    var x = 10
    func doSomething() {
        // 使用到前面定義的兩個函式 都使用了尾隨閉包來讓語法更為簡潔
        // 傳入當參數的閉包 內部都是將實體的屬性指派為新的值
        someFunctionWithEscapingClosure { self.x = 100 }
        // 可以看到這個標註 @noescape 的參數的閉包 其內可以隱式的參考 self
        someFunctionWithNoescapeClosure { x = 200 }
    }
}

// 生成一個實體
let instance = SomeClass()

// 呼叫其內的方法
instance.doSomething()
// 接著那兩個前面定義的函式都會被呼叫到 所以最後實體的屬性 x 為 200
print(instance.x)

// 接著呼叫陣列中的第一個成員
// 也就是示範逃逸閉包的函式中 會將閉包加入陣列的這個動作
// 而這個第一個成員就是 { self.x = 100 }
completionHandlers.first?()
// 所以這時實體的屬性 x 便為 100
print(instance.x)

```


### 自動閉包

自動閉包(`autoclosure`)是一種自動被建立的閉包，用於包裝後傳遞給函式作為參數的表達式。這種閉包沒有參數，而當被使用時，會返回被包裝在其內的表達式的值。

也就是說，自動閉包是一種簡化的語法，讓你可以用一個普通的表達式代替顯式的閉包，進而省略了閉包的大括號`{}`。

自動閉包讓你可以延遲求值，因為這個閉包會直到被你呼叫時才會執行其內的程式，以下先示範一個普通的閉包如何延遲求值：

```swift
// 首先宣告一個有五個成員的陣列
var customersInLine = ["Albee", "Alex", "Eddie", "Zack", "Kevin"]

// 印出：5
print(customersInLine.count)

// 接著宣告一個閉包 會移除掉陣列的第一個成員
let customerProvider = { customersInLine.removeAtIndex(0) }

// 這時仍然是印出：5
print(customersInLine.count)

// 直到這個閉包被呼叫時 才會執行
// 印出：開始移除 Albee ！
print("開始移除 \(customerProvider()) ！")

// 這時就只剩下 4 個成員了
print(customersInLine.count)

```

上述程式可以看到閉包直到被呼叫時，才會移除成員，所以如果不呼叫閉包的話，則陣列成員都不會被移除。另外要注意一點，這個閉包`customerProvider`的型別為`() -> String`，而不是`String`。

將閉包作為參數傳遞給函式時，一樣可以延遲求值，如下：

```swift
// 這時 customersInLine 為 ["Alex", "Eddie", "Zack", "Kevin"]

// 定義一個閉包作為參數的函式
func serveCustomer(customerProvider: () -> String) {
    // 函式內部會呼叫這個閉包
    print("開始移除 \(customerProvider()) ！")
}

// 呼叫函式時 [移除陣列第一個成員]這個動作被當做閉包的內容
// 閉包被當做參數傳入函式
// 這時才會移除陣列第一個成員
serveCustomer( { customersInLine.removeAtIndex(0) } )

```

接著則介紹如何使用自動閉包完成上述一樣的動作。你必須在參數前面標註`@autoclosure`，以表示這個參數可以是一個自動閉包的簡化寫法，這時就可以將該函式當做接受`String`型別參數的函式來呼叫。這個前面標註`@autoclosure`的參數會將自己轉換成一個閉包，如下：

```swift
// 這時 customersInLine 為 ["Eddie", "Zack", "Kevin"]

// 這個函式的參數前面標註了 @autoclosure 表示這參數可以是一個自動閉包的簡化寫法
func serveCustomer(@autoclosure customerProvider: () -> String) {
    print("開始移除 \(customerProvider()) ！")
}

// 因為函式的參數有標註 @autoclosure 這個參數可以不用大括號 {}
// 而僅僅只需要[移除第一個成員]這個表達式 而這個表達式會返回[被移除的成員的值]
serveCustomer(customersInLine.removeAtIndex(0))

```

自動閉包含有**非逃逸閉包**的特性，所以你如果想讓這個閉包可以**逃逸**
，則必須標註為`@autoclosure(escaping)`，以下是一個例子：

```swift
// 這時 customersInLine 為 ["Zack", "Kevin"]

// 宣告另一個變數 為一個陣列 其內成員的型別為 () -> String
var customerProviders: [() -> String] = []

// 定義一個函式 參數標註 @autoclosure(escaping) 表示參數是一個可逃逸自動閉包
func collectCustomerProviders(@autoclosure(escaping) customerProvider: () -> String) {
    // 函式內部的動作是將當做參數的這個閉包 再加入新的陣列中 因為可逃逸 所以不會出錯
    customerProviders.append(customerProvider)
}

// 呼叫兩次函式 會將 customersInLine 剩餘的兩個成員都轉加入新的陣列中
collectCustomerProviders(customersInLine.removeAtIndex(0))
collectCustomerProviders(customersInLine.removeAtIndex(0))

// 印出：獲得了 2 個成員
print("獲得了 \(customerProviders.count) 個成員")

// 最後將這兩個成員也從新陣列中移除
for customerProvider in customerProviders {
    print("開始移除 \(customerProvider()) ！")
}
// 依序印出：
// 開始移除 Zack ！
// 開始移除 Kevin ！

```

