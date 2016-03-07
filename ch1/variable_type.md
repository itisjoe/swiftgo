# 變數及常數的類型

依據不同需求，變數會需要不同的類型來執行動作，像是身高體重需要有小數點的數字，計算人數需要整數，姓名、名稱需要文字。

### Int 整數

整數指的是沒有小數點的數字，可以**有符號 (正、負、零)**或是**無符號 (正、零)**。

```swift
let number = 12
var anotherNumber = 24

```

### Float 及 Double 浮點數

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