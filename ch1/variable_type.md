# 變數及常數的型別

依據不同需求，變數會需要不同的型別來執行動作，像是身高體重需要有小數點的數字，計算人數需要整數，姓名、名稱需要文字字串。

### 整數

整數(`Int`)指的是沒有小數點的數字，可以**有符號 (正、負、零)**或是**無符號 (正、零)**。

Swift 提供 8 、 16 、 32 和 64 位元的有符號的整數型別，依序為`Int8`、`Int16`、`Int32`和`Int64`，以及無符號的整數型別，依序是`UInt8`、`UInt16`、`UInt32`和`UInt64`。

另外也提供一個特殊的整數型別`Int`，這個型別的長度與目前平台的原生字長相同，在 32 位元平台與`Int32`相同，在 64 位元平台則與`Int64`相同。

所以通常我們使用整數型別時，使用`Int`即可。

```swift
let number = 12
var anotherNumber = -240

```

##### Hint

- 還有一個特殊的**無符號**整數型別為`UInt`，這個型別的長度與目前平台的原生字長相同。但基於程式碼的可重複性，避免不同型別數字之間的轉換，以及數字的型別推斷，大部分情況都推薦只使用`Int`即可。


### Float, Double 浮點數

浮點數指的是有包含小數點的數字， Float 跟 Double 的差別在於精確度， Float 有 6 位數，而 Double 可以達到 15 位數，選擇使用哪一個則是看你程式需要處理值的範圍而定。

```swift
let pi = 3.1415926
var height = 178.25

```


### 整數和浮點數轉換

整數和浮點數的轉換必須指定型別。

```swift
// 型別為整數 Int
let number1 = 3

// 型別為浮點數 Double
let number2 = 0.1415926

// 相加前 需要將 Int 轉換成 Double 否則會報錯誤
let pi = Double(number1) + number2

// 這個值的型別也就是 Double
print(pi) // 印出 3.1415926
```

相反來說也行，可以將浮點數轉換成整數，但小數點後的數字就會被截斷。

```swift
let integerPi = Int(pi)

// 型別為 Int 小數點後的數字被截斷 所以只會印出 3
print(integerPi) // 印出 3

```


### Bool 布林值

布林值指的是邏輯上的值，只能為真或假。 Swift 有兩個布林常數：`true`跟`false`。

```swift
let storeOpen = true
let forFree = false

```


### Character, String 字元及字串

字元指的是依照編碼格式的一個單位元組，而字串是有序的字元集合(簡單說就是一段文字)，皆是以兩個雙引號`"`前後包起來。

```swift
let firstString = "Nice to meet you."
let secondString = "Nice to meet you,too."

```

如果要在字串中加入其他變數或常數，要使用 `\()` 這個方式。

```swift
let score = 80
let string = "My score is \(score) ."
print(string) // 印出 My score is 80 .

```

更多字串的操作會在後面章節中繼續講解。


### Tuples 元組

元組是將多個值組合成一個複合值，其內的型別可以不同，以小括號`()`前後包起來，每個值以逗號`,`分隔。

```swift
// 宣告一個元組並填值進去 依序是字串、整數、浮點數
let myInfo = ("Kevin Chang", 25, 178.25)

```

要使用其中一個值，可以依照順序的索引值取得 (這裡的順序從 0 開始算起，接著依序 1,2,3 ...)

```swift
// 取得前面宣告的 myInfo 的第三個值 因為是從 0 開始算 所以是 2
let myHeight = myInfo.2
print("My height is \(myHeight)") // 印出 My height is 178.25

```

也可將一個元組分解成單獨的常數或變數。

```swift
// 將前面宣告的 myInfo 分解成三個常數
let (myName, myAge, myHeight) = myInfo
print("My name is \(myName) .") // 印出 My name is Kevin Chang .
print("I am \(myAge) years old .") // 印出 I am 25 years old . 

```

如果只需要其中某些值時，分解時可以把不需要的用底線 `_` 標記

```swift
let (_, _, myHeight) = myInfo
print("My height is \(myHeight) .") // 印出 My height is 178.25 .


```

或是在宣告元組時就個別給裡面的值一個名稱也可以

```swift
let herInfo = (name:"Jess", age:24, height:160.5)

// 除了用順序取得外 如果有設定名稱 也可以直接取用
print("Her name is \(herInfo.name) . ") // 印出 Her name is Jess .

```


### Type annotation 型別標註

宣告變數或常數時，可以加上型別標註，說明這個值的型別。使用方法是在值的後面加上冒號 `:` 接著加上型別名稱。

```swift
// 宣告一個整數變數
var number: Int

// 宣告一個字串常數
let str: String = "It is a string ."

```

通常很少需要寫型別標註，如果在宣告時給了一個初始值， Swift 則會自動推斷出型別。不過有些情況可能仍然需要寫。

```swift
// 宣告浮點數時 如果沒有型別標註 通常會將他判斷為 Double 
let height = 165.25 // 型別為 Double
let anotherHeight: Float = 175.5 // 除非型別標註填寫為 Float

// 宣告字串時 不論字數多少 都會判斷為 String
let str1 = "It is a string ." // 型別為 String
let str2 = "b" // 型別仍然是 String
let str3: Character = "c" // 除非型別標註填寫為 Character

```


### 型別別名

型別別名(`type aliases`)就是給已存在的型別定義另一個名字，必須使用關鍵字`typealias`來定義型別別名。當你想要給已存在的型別命名一個更有意義的名字時很有用，底下是一個例子：

```swift
// 將 Int 型別定義一個新的名字 MyType
typealias MyType = Int

// 這時就可以宣告一個 MyType 變數 其實也就是 Int 變數
var number: MyType = 123

```


### Optionals 可選型別

這是 Swift 的一個特性，讓變數或常數可以有**沒有值**的情況，這與零`0`或是空字串`""`不同，當沒有值時，變數或常數會返回`nil`。而`nil`代表的就是**沒有值**，任何型別只要有加上`可選型別`都可以設置成`nil`。使用方法如下：

```swift
// 在宣告變數時 型別標註後面加上一個問號 ?
var score: Int? // 因為目前尚未指派 所以目前 score 會被設置成 nil 也就是沒有值的狀態
score = 100 // 設值為 100
score = nil // 再將變數設為 nil 目前又是沒有值的狀態

var totalScore: Int // 但如果沒有加上 ? 則是尚未指派的狀態 這時如果直接使用會報錯誤
totalScore = nil // 也不能設成 nil 這行同樣也會報錯誤

// 宣告常數也是一樣
let myName: String?

```

以下是一個例子，來說明可能會遇到的可選型別的情況。

```
let number = "5566" // 宣告一個字串常數

let newNumber = Int(number) // 嘗試將這個字串轉換成整數

```

這時如果原字串內容不是單純的數字，轉換後則是會返回`nil`，來避免發生錯誤的情況。也就是說返回的是一個可選型別`Int?`，他可能會是整數，也可能是`nil`。


### Forced unwrapping 強制解析

當你確認一個**可選型別**一定有值，則可以在這個變數後面加上一個驚嘆號`!`，表示**這個可選型別有值，請使用它**。

```swift
let number: Int? = 500 // 宣告一個整數常數 並賦值
print(number!) // 以這個例子來說 常數確實有值 所以加上驚嘆號 表示這個可選型別有值 可以直接使用

var number2: Int? // 尚未賦值 所以目前是 nil
print(number2!) // 仍然要使用的話 這行則會報錯誤
```


### Implicitly unwrapped optionals 隱式解析可選型別

當**可選型別**第一次被賦值後，如果可以確定他之後都會有值，這時可以將其改為**隱式解析可選型別**，這樣便不需要每次都判斷及解析，作法則是將可選型別的問號`?`改成驚嘆號`!`，如下說明

```swift
let firstString: String? = "Good morning ." // 可選型別
let anotherString: String = firstString! // 需要驚嘆號來取值

let secondString: String! = "Good night ." // 如果改成隱式解析可選型別
let finalString: String = secondString // 則可以直接使用

```
