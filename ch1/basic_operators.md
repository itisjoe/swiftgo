# 基本運算子

運算子是檢查、改變、合併值的特殊符號或短語。像是加號`+`就是將兩個數相加 (`let number = 1 + 2`)，或是直接讓`i`加 1 的自加運算子`++i`。


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

#### 餘數運算

餘數運算( `a % b` )是計算`b`的多少倍剛剛好可以容入`a`，返回多出來的那部分，也就是餘數。

`a = (b * 倍數) + 餘數`

以下的例子為`9 = (4 * 2) + 1`，`9`等於`4`乘上倍數`2`再加上餘數`1`。返回的值就是餘數，也就是`1`。

```swift
var number = 9 % 4
print(number) // 餘數等於 1

```

Swift 比較特別的一點是，浮點數也可以取餘數：

```swift
var number = 8.0 % 2.5 // 8.0 = (2.5 * 3.0) + 0.5
print(number) // 餘數等於 0.5

```

#### 自加和自減運算

如果需要自加或自減`1`時，有個簡潔的運算子`++`及`--`，代表的是對這個變數加`1`或減`1`。

```swift
var i = 1
++i // 因為 i 加 1 所以現在 i 等於 2
--i // i 減 1 現在 i 等於 1

```

`++i`也就是`i = i + 1`的簡寫，而`--i`是`i = i - 1`的簡寫。

`++`及`--`既是前綴也是後綴運算子，`++i`、`i++`、`--i`、`i--`都是有效的寫法。前綴與後綴的差別在於返回值的不同，當`++`前綴時，這個變數會先自加再返回，`++`後綴時，則是先返回再自加，例子如下：

```swift
// ++ 前綴
var a = 3
var b = ++a // a 先自加才返回 所以現在 a 跟 b 都是 4

// ++ 後綴
var c = 5
var d = c++ // c 先返回才自加 所以 c 等於 6, d 等於 c 自加前的數值 5

// -- 也是一樣的規則

```

除非需要`i++`的特性，不然建議使用`++i`及`--i`，先自加或自減後才返回，比較符合使用上的邏輯。


### 複合賦值運算子

Swift 提供一個簡潔的方式，將其他運算子與賦值運算合併，像是`+=`，很多時候可以簡化程式。

```swift
var a = 3
a += 2 // 這行等同於 a = a + 2 的簡寫
print(a) // 現在 a 等於 5

// 其他運算子也是一樣
a -= 4 // a = a - 4 , 現在 a 等於 1
a *= 10 // a = a * 10 , 現在 a 等於 10
a /= 2 // a = a / 2 , 現在 a 等於 5
a %= 2 // a = a % 2 , 現在 a 等於 1

```


### 比較運算子

將兩個數值作比較，並返回這個比較是否成立的布林值，即返回`true`或是`false`，以下是常用的比較運算。

- 等於（`a == b`）
- 不等於（`a != b`）
- 大於（`a > b`）
- 小於（`a < b`）
- 大於等於（`a >= b`）
- 小於等於（`a <= b`）

```swift
1 == 1 // 返回 true 因為 1 等於 1
2 != 1 // 返回 true 因為 2 不等於 1
2 > 1 // 返回 true 因為 2 大於 1
1 < 2 // 返回 true 因為 1 小於2
1 >= 1 // 返回 true 因為 1 大於等於 1
2 <= 1 // 返回 false 因為 2 不小於等於 1

```

常用於條件語句，像是`if`條件：

```swift
var i = 1
if i == 1 {
    print("Yes, it is 1 .")
} else {
    print("No, it is not 1 .")
}

// 因為 i 等於 1, 返回 true, 所以會印出 Yes, it is 1 .

```

後面章節會正式介紹`if`的使用方法。

### 三元運算子

這是一個簡潔的條件式運算：`問題 ? 答案1 : 答案2`。`問題`需要返回一個是否成立的布林值，表示`true`或`false`，如果為`true`，則是返回`答案1`，反之如果為`false`，則是返回`答案2`。也就是下列寫法的簡寫：

```swift
if 問題 {
    答案1
} else {
    答案2
}

```

以下是個例子：

```swift
var score = 25
if score < 60 {
    score = score + 50
} else {
    score = score + 20
}

print(score) // 現在 score 等於 75

```

使用三元運算子可以簡化成下面這樣：

```swift
var score = 25
score = score + (score < 60 ? 50 : 20)

```

後面章節會正式介紹`if`的使用方法。


### 空值聚合運算子

前面章節介紹過的可選型別， Swift 提供一個簡潔的使用方法：`a ?? b`。先判斷`a`是否為`nil`，如果`a`有值，不是`nil`，就會解析`a`並返回，但如果`a`為`nil`，則返回預設值`b`。也就是下面這個寫法的簡寫：
```swift
a != nil ? a! : b

```

這裡用到前面提到的三元運算子，如果`a`不等於`nil`，則強制解析`a`並返回，否則就返回`b`。以下為一個例子：

```swift
let defaultColor = "red"
var userDefinedColor: String? // 未賦值 所以預設為 nil
var colorToUse = userDefinedColor ?? defaultColor
print(colorToUse) // 未賦值給 userDefinedColor ,所以會返回 defaultColor, 這邊即印出 red

// 反之如果有賦值
var userAnotherDefinedColor: String? = "green"
var anotherColorToUse = userAnotherDefinedColor ?? defaultColor
print(anotherColorToUse) // 這邊即印出 green

```


### 區間運算子

### 邏輯運算子

### 括號優先