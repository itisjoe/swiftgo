# 變數及常數的型別

依據不同需求，變數會需要不同的型別來執行動作，像是身高體重需要有小數點的數字，計算人數需要整數，姓名、名稱需要文字字串。

### Int 整數

整數指的是沒有小數點的數字，可以**有符號 (正、負、零)**或是**無符號 (正、零)**。

```swift
let number = 12
var anotherNumber = -240

```


### Float, Double 浮點數

浮點數指的是有包含小數點的數字， Float 跟 Double 的差別在於精確度， Float 有 6 位數，而 Double 可以達到 15 位數，選擇使用哪一個則是看你程式需要處理值的範圍而定。

```swift
let pi = 3.1415926
var height = 178.25

```


### Bool 布林值

布林值指的是邏輯上的值，只能為真或假。 Swift 有兩個布林常數：`true`跟`false`

```swift
let storeOpen = true
let forFree = false

```


### Character, String 字元及字串

字元指的是依照編碼格式的一個單位元組，而字串是有序的字元集合，皆是以兩個雙引號 `"` 前後包起來

```swift
let firstString = "Nice to meet you."
let secondString = "Nice to meet you,too."

```

如果要在字串中加入其他變數或常數，要使用 `\()` 這個方式

```swift
let score = 80
let string = "My score is \(score) ."
print(string) // 印出 My score is 80 .

```

更多字串的操作會在後面章節中繼續講解。


### Tuples 元組

元組是將多個值組合成一個複合值，其內的型別可以不同，以小括弧 `()` 前後包起來，每個值以逗號 `,` 分隔。

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

你也可將一個元組分解成單獨的常數或變數。

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

通常很少需要寫型別標註，如果在宣告時給了一個初始值， Swift 則會自動判斷出型別。不過有些情況可能仍然需要寫。

```swift
// 宣告浮點數時 如果沒有型別標註 通常會將他判斷為 Double 
let height = 165.25 // 型別為 Double
let anotherHeight: Float = 175.5 // 除非型別標註填寫為 Float

// 宣告字串時 不論字數多少 都會判斷為 String
let str1 = "It is a string." // 型別為 String
let str2 = "b" // 型別仍然是 String
let str3: Character = "c" // 除非型別標註填寫為 Character


```












