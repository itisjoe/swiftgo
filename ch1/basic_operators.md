# 基本運算子

運算子是檢查、改變、合併值的特殊符號或短語。像是加號`+`就是將兩個數相加 (`let number = 1 + 2`)，或是直接讓`i`加 1 的累加運算子`++i`。


### 賦值運算子

前面章節已經有多次使用，`a = b`表示將右邊的`b`賦值給左邊的`a`。

```swift
let b = 10
var a = 5
a = b // 將 b 賦值給左邊的 a
print(a) // 現在 a 等於 10

```


### 數值運算子

Swift 中所有數值型別都支援基本的四則運算，加 `+`、減 `-`、乘 `*`、除 `/` 。

```swift
var a = 1 + 2 // a 等於 3
var b = 7 - 2 // b 等於 5
var c = 3 * 2 // c 等於 6
var d = 10.0 / 2.5 // d 等於 4.0

```

加法運算子也可以用於字串的合併：

```swift
let firstString = "Hello, "
let secondString = "world."
let finalString = firstString + secondString
print(finalString) // 印出 Hello, world.

```

後面章節會介紹更多字串的操作。


### 複合賦值運算子

### 比較運算子

### 三元運算子

### 空值聚合運算子

### 區間運算子

### 邏輯運算子

### 括號優先