# 基本運算子

- [指派運算子](#assignment)
- [數值運算子](#arithmetic)
- [複合指派運算子](#compound_assignment)
- [比較運算子](#comparison)
- [三元運算子](#ternary_conditional)
- [空值聚合運算子](#nil_coalescing)
- [區間運算子](#range)
- [邏輯運算子](#logical)
- [括號優先](#explicit_parentheses)

運算子是檢查、改變、合併值的特殊符號或語句。像是加號`+`就是將兩個數相加 (`let number = 1 + 2`)。

<a name="assignment"></a>
### 指派運算子

`a = b`表示將右邊的`b`指派給左邊的`a`。

```swift
let b = 10
var a = 5
// 將 b 指派給左邊的 a
a = b
// 現在 a 等於 10

// 你也可以直接指派元組 會直接分解為多個常數或變數
let (x, y) = (1, 2)
// 現在 x 為 1, y 為 2

```

<a name="arithmetic"></a>
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

// 印出：Hello, world.
print(finalString)

```

後面章節會介紹更多[字串](../ch1/strings_characters.md)的操作。

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

#### 一元負號

數值的正負號可以使用前綴`-`（即一元負號）來切換：

```swift
let number = 3
var anotherNumber = -number // 為 -3
var finalNumber = -anotherNumber // 為 3

```

#### 一元正號

一元正號`+`則是不做任何改變地回傳數值。

```swift
let number = -6
var anotherNumber = +number // 為 -6

```

<a name="compound_assignment"></a>
### 複合指派運算子

Swift 提供一個簡潔的方式，將數值運算與指派運算合併，像是`+=`，很多時候可以簡化程式。

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

<a name="comparison"></a>
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

// 因為 i 等於 1, 返回 true, 所以會印出：Yes, it is 1 .

```

後面章節會正式介紹 [if 的使用方法](../ch1/control_flow.md#condition)。

當元組中的值可以比較時，你也可以用比較運算子來比較它們的大小。像是`Int`和`String`型別的值可以比較，所以元組`(Int, String)`也可以被比較。而`Bool`不能比較，所以內含布林值型別的元組不能被比較。

元組比較大小會依序由左到右逐個比較，直到有兩個值不相等為止。而如果所有值都相等，則會將這一對元組稱為相等的。例子如下：

```swift
// true 因為 1 小於 2
(1, "zebra") < (2, "apple")

// true 因為 3 等於 3,但是 apple 小於 bird
(3, "apple") < (3, "bird")

// true 因為 4 等於 4,dog 等於 dog
(4, "dog") == (4, "dog")

```

##### Hint

- Swift 在比較元組的成員時，限制最多只能比較六個成員，如果有七個或七個以上成員則無法比較。

<a name="ternary_conditional"></a>
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

<a name="nil_coalescing"></a>
### 空值聚合運算子

前面章節介紹過的[可選型別](../ch1/types.md#optional_type)， Swift 提供一個簡潔的使用方法：`a ?? b`。先判斷`a`是否為`nil`，如果`a`有值，不是`nil`，就會解析`a`並返回，但如果`a`為`nil`，則返回預設值`b`。也就是下面這個寫法的簡寫：
```swift
a != nil ? a! : b

```

這裡用到前面提到的三元運算子，如果`a`不等於`nil`，則強制解析`a`並返回，否則就返回`b`。以下為一個例子：

```swift
let defaultColor = "red"
var userDefinedColor: String? // 未賦值 所以預設為 nil
var colorToUse = userDefinedColor ?? defaultColor
// 未賦值給 userDefinedColor ,所以會返回 defaultColor
// 這邊即印出：red
print(colorToUse)

// 反之如果有賦值
var userAnotherDefinedColor: String? = "green"
var anotherColorToUse = userAnotherDefinedColor ?? defaultColor
// 這邊即印出：green
print(anotherColorToUse)

```

<a name="range"></a>
### 區間運算子

Swift 提供兩個方便表達一個區間的值的運算子。

#### 閉區間運算子

表示方式為：`a...b`，定義一個包含從`a`到`b`(包括`a`和`b`)的所有值的區間。`b`必須大於等於`a`。

```swift
// 1...5 代表的就是 1,2,3,4,5 這五個數字
for index in 1...5 {
    print("\(index) * 5 = \(index * 5)")
}
// 依序印出
// 1 * 5 = 5
// 2 * 5 = 10
// 3 * 5 = 15
// 4 * 5 = 20
// 5 * 5 = 25

```

後面章節會正式介紹 [for-in 的使用方法](../ch1/control_flow.md#for)，這邊先理解為是一個會依照規則依序執行動作的語法。

#### 半開區間運算子

表示方式為：`a..<b`，定義一個從`a`到`b`但不包括`b`的區間。`b`必須大於等於`a`，但當`a`等於`b`時，則不會有值。

```swift
// 1..<5 代表的就是 1,2,3,4 這四個數字 不包括 5
for index in 1..<5 {
    print("\(index) * 5 = \(index * 5)")
}
// 依序印出
// 1 * 5 = 5
// 2 * 5 = 10
// 3 * 5 = 15
// 4 * 5 = 20

```

<a name="logical"></a>
### 邏輯運算子

Swift 支援三個標準邏輯運算。

- 邏輯非（`!a`）
- 邏輯且（`a && b`）
- 邏輯或（`a || b`）

`a`及`b`都是邏輯布林值，且皆會返回一個邏輯布林值，即`true`或是`false`。

#### 邏輯非

`!a`對一個布林值取相反值，即將`true`變`false`，或是將`false`變`true`。這是一個前綴運算子，且不加空格，例子如下：

```swift
let isOpen = false
if !isOpen {
    print("It is open .")
}

```

#### 邏輯且

`a && b`表示只有當`a`跟`b`都為`true`時，才會返回`true`，否則如果`a`或`b`其中一個為`false`，就會返回`false`，如下：

```swift
let isOpen = true
let isWeekend = false
if isOpen && isWeekend {
    print("Success !")
} else {
    print("Failure !")
}
// 因為其中一個為 false 所以會返回 false
// 即印出：Failure !

```

#### 邏輯或

`a || b`表示只要`a`跟`b`其中一個值為`true`時，就會返回`true`，除非`a`和`b`皆為`false`，才會返回`false`，如下：

```swift
let isSunday = true
let isWeekend = false
if isSunday || isWeekend {
    print("Success !")
} else {
    print("Failure !")
}

// 因為其中一個為 true 就會返回 true
// 即印出：Success !

```

<a name="explicit_parentheses"></a>
### 括號優先

以上介紹很多運算子，當一個運算式太複雜時，可以使用括號`()`來標示清楚，同時也用來表明優先級(如同傳統學習數學計算一樣，括號括起來的部份要優先計算)。例子如下：

```swift
// 數值運算
var number = 3 + 2 * 5 // 先乘除後加減 所以 number 等於 13
var anotherNumber = (3 + 2) * 5 // 括號括起來的優先 所以 anotherNumber 等於 25

// 邏輯運算
let isOpen = false
let isWeekend = true
let isSunday = true

// 由左至右依序判斷
if isOpen && isWeekend || isSunday {
    print("Success !")
} else {
    print("Failure !")
}
// 先作"邏輯且"判斷 isOpen && isWeekend 會返回 false
// 再與後面的 isSunday 作"邏輯或"的判斷 會返回 true
// 所以這邊會印出：Success !

// 括號有優先權
if isOpen && (isWeekend || isSunday) {
    print("Success !")
} else {
    print("Failure !")
}
// 括號優先 所以先做"邏輯或"判斷 isWeekend || isSunday 會返回 true
// 再與前面的 isOpen 作"邏輯且"的判斷 會返回 false
// 所以這邊會印出：Failure !

```

