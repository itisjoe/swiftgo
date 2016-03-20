# 閉包 Closures

閉包(`closure`)簡單來講就是一個匿名函式，同樣是一個獨立的程式區塊。像是前面章節提到的嵌套函式(nested functions)就是一種閉包，可以在程式中被傳遞和使用。

閉包有三種表現方式：

- **函式**就是一種有名稱的閉包。
- **嵌套函式**就是一種有名稱且被包含在另一個函式中的閉包。
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

