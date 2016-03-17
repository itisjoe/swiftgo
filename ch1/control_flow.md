# 控制流程

Swift 提供了可以循環執行任務的`for`和`while`迴圈，以及根據條件選擇執行不同程式碼分支的`if`和`switch`語句，還有控制流程跳轉到其他程式碼的`break`和`continue`語句。

### For 循環

Swift 提供兩種`for`循環方式，按照指定的次數執行任務。

- `for-in` 循環對一個集合(像是陣列`Array`、字典`Dictionary`)遍歷其內所有的元素，操作此元素並執行循環內的程式。
- `for` 循環會重複執行內部的程式，直到設定的條件達成，通常會在每次循環完成後增加計數器的值來實現。

#### For-in

使用`for-in`遍歷一個集合內的所有元素，像是一個數字區間、陣列中的值或是字串內的字元。

```swift
// 一個一到三的閉區間
for index in 1...3 {
    print(index)
}
// 會依序印出
// 1
// 2
// 3
```

上述程式內的`index`不用做宣告的動作，在每次遍歷開始時會被自動指派值。

另外如果只是要純粹的循環，不需要用到區間內的每一個值，可以用下底線`_`來代替。

```swift
let base = 2
var total = 1
for _ in 1...3 {
    total *= base
}
print(total) // 印出 8, 因為循環了三次 所以乘了三次

```

使用`for-in`遍歷一個陣列(`Array`)或是字典(`Dictionary`)。遍歷字典時，會使用元組(`Tuple`)來分別表示鍵與值。

```swift
let arr = ["Apple", "Book", "Cat"]
for n in arr {
    print(n)
}
// 依序印出
// Apple
// Book
// Cat

let dict = ["Apple":12, "Book":3, "Cat":5]
for (key, values) in dict {
    print("\(key) : \(values)")
}
// 印出 (因為字典沒有順序 所以不一定是這樣的順序)
// Apple : 12
// Book : 3
// Cat : 5

```

#### For

這種循環的格式如下：

```swift
for 初始化;條件表達式;計數器計數 {
    每次循環執行的程式
}

```

循環流程：

1. 循環第一次啟動時，會初始化循環所需要的常數和變數，這只會被執行一次。
2. 每次循環都會先看條件表達式，如果這個條件表達式返回`false`，則不會執行循環並結束，會繼續執行`}`之後的程式。反之如果返回`true`，則會執行一次`{}`內部程式。
3. 執行完一次內部程式後，會對計數器計數，可能是增加或減少值，或是根據內容修改某一個初始化的變數。接著則重複執行第 2 步。

```swift
// 首先初始化一個變數 index 為 0
// 條件表達式為 index < 3 第一次執行時會返回 true 因為 0 < 3
// 接著會執行內部程式 這邊是一個 print()
// 完成後做計數的動作 對 index 加 1 現在 index 為 1
// 然後重複動作 繼續檢查條件表達式 
// 直到 index 為 3 會導致條件表達式返回 false 則結束循環
for var index = 0; index < 3; ++index {
    print("index is \(index)")
}
// 會依序印出
// index is 0
// index is 1
// index is 2

```


### While 循環

Swift 提供兩種`while`循環方式：`while`及`repeat-while`，兩者都是循環地執行程式直到條件表達式返回`false`，兩者的差別在於，後者一開始在檢查條件表達式之前，一定會先執行一次內部程式。

#### While

`while` 循環格式如下：

```swift
while 條件表達式 {
    每次循環執行的程式
}

```

`while` 會循環地執行程式直到條件表達式返回`false`。

```swift
var n = 2
while n < 20 {
    n = n * 2
}
print(n) // 印出 32
```

#### Repeat-while

`repeat-while` 循環格式如下：

```swift
repeat {
    每次循環執行的程式
} while 條件表達式

```

`repeat-while` 會先執行一次程式，再檢查條件表達式，接著循環地執行程式直到條件表達式返回`false`。

```swift
var n = 512
repeat {
    n = n * 2
} while n < 100
print(n) // 印出 1024
// 因為不論如何 都會先執行一次程式 所以 n 會先乘一次 2 為 1024
// 接著檢查條件表達式 會返回 false 即結束這個循環

```


### 條件語句

條件語句為根據不同特定條件執行不同的程式。Swift 提供兩種條件語句：`if`與`switch`，如果需要判斷的條件較單純或可能的情況較少時，可以使用`if`，反之使用`switch`。

#### If

最簡單的形式為只有一個條件表達式，而只有當這個條件表達式返回`true`，才執行其內的程式。

```swift
let number = 2
if number == 2 {
    print("It is 2 .")
}

```

或是當條件表達式返回`false`時，需要執行另一段程式，則使用`else`。

```swift
let number = 10
if number > 20 {
    print ("Number is larger than 20 .")
} else {
    print ("Number is smaller than 20 or equal to 20 .")
}
// 印出 Number is smaller than 20 or equal to 20 .

```

又或是有多個條件需要判斷，可以將`else`跟`if`連在一起，最後一個`else`會在所有條件都不成立(返回`false`)時被執行。

```swift
let number = 100
if number < 20 {
    print ("Number is smaller than 20 .")
} else if number < 200 {
    print ("Number is not smaller than 20 but smaller than 200 . ")
} else if number < 1000 {
    print ("Number is not smaller than 200 but smaller than 1000 . ")
} else {
    print ("Number is not smaller than 1000 .")
}
// 印出 Number is not smaller than 20 but smaller than 200 . 

```

上述程式中最後一個`else`不是一定要有，也可以省略，但就可能都沒有返回`true`的條件表達式。

```swift
let number = 10
if number > 50 {
    print("number > 50")
} else if number > 200 {
    print("number > 200")
}
// 這時候不會有東西被印出來

```

### 可選綁定 optional binding

使用可選綁定來判斷可選型別是否有值，如果有值的話就指派給一個臨時常數或臨時變數，並執行其內部的程式，這個臨時常數或變數只能在其內部使用。`if`跟`while`語句都可以使用可選綁定。格式如下：

```swift
if let 臨時常數 = 可選型別 {
    執行的程式
} else {
    可選型別沒有值 也就是 nil 時 執行的程式
}

```

以下是一個例子：

```swift
// 字串內容是純整數 所以經過轉換後會是一個整數
let str = "123"
if let number = Int(str) {
    print("字串\"\(str)\" 轉換成一個整數 \(number)")
} else {
    print("字串\"\(str)\" 不是一個整數")
}

// 如果字串內容不是整數 轉換後會返回 nil 
let str2 = "It is a string ."
if let number = Int(str2) {
    print("字串\"\(str2)\" 轉換成一個整數 \(number)")
} else {
    print("字串\"\(str2)\" 不是一個整數")
}

```


### Switch

`switch`會將一個值與多個情況作比對，根據第一個比對成功的情況，會執行相對應的程式。最簡單的格式如下：

```swift
switch 值 {
case 比對情況1:
    相對應情況1 執行的程式
case 比對情況2, 比對情況3: // 多個情況可以用逗號 , 隔開
    相對應情況2或情況3 執行的程式
default:
    以上情況比對都不成功時 執行的程式
}

```

每個`case`中，都一定要有相對應執行的程式，如果沒有的話會報錯誤。

```swift
let number = 2
switch number {
    case 1:
    case 2:
        print("It is 2 .")
    case 3:
    default:
        print("沒有比對到")
}
// case 1 以及 case 3 內部都沒有執行的程式 所以這邊會報錯誤

```

##### Hint

與其他程式語言不一樣的是，有些程式語言的`switch`需要在每個`case`內的程式最後一行加上`break`來結束，否則會繼續執行底下其他`case`中的程式直到`default`之前。

Swift 在遇到第一個情況`case`比對成功後，即會結束`switch`這部份的動作，繼續執行`}`之後的程式。(除非使用`fallthrough`，稍後即會提到。)

#### 區間匹配 range matching

`case`中比對的情況也可以是一個區間。

```swift
let number = 120
var str: String
switch number {
    case 0...10:
        str = "幾"
    case 11...100:
        str = "很多"
    case 101...1000:
        str = "非常多"
    default:
        str = "超級多"
}
print("我有\(str)顆蘋果")
// 印出 我有非常多顆蘋果 因為 120 在 101...1000 這個區間內

```





```
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
```






