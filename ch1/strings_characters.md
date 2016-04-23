# 字串及字元

- [字串字面量](#string_literal)
- [初始化空字串](#initializing_an_empty_string)
- [字串可變性](#string_mutability)
- [使用字元](#working_with_characters)
- [連接字元及字串](#concatenating_strings_characters)
- [字串插值](#string_interpolation)
- [特殊符號](#special_characters)
- [計算字串中的字元數量](#counting_characters)
- [比較字串](#comparing_strings)

字元指的是依照編碼格式的一個單位元組，而字串是有序的字元集合(簡單說就是一段文字)，皆是以兩個雙引號`"`前後包起來。

<a name="string_literal"></a>
### 字串字面量

在程式碼中包含一段預先定義的字串值作為字串字面量(`string literal`)。字串字面量是由一對雙引號`""`包著的具有固定順序的文字字元集合，可以為常數和變數提供初始值。

```swift
let someString = "Some string literal value"

```

<a name="initializing_an_empty_string"></a>
### 初始化空字串

將空的字串字面量指派給變數，或是也可以初始化一個新的`String`變數：

```swift
// 這兩個是一樣的意思
var emptyString = ""
var anotherEmptyString = String()

```

<a name="string_mutability"></a>
### 字串可變性

將一個特定的字串指派給一個變數，之後還可以對其修改。而字串指派給一個常數，則無法再做修改，例子如下：

```swift
var variableString = "Cat"
variableString = "Book"
// variableString 現在為 Book

let constantString = "Sun"
constantString = "Moon" // 這行會報錯誤 因為常數不能被修改

```

<a name="working_with_characters"></a>
### 使用字元

字串是有序的字元集合，所以可以使用`for-in`迴圈來遍歷字串中的每一個字元：

```swift
for character in "Dog!".characters {
    print(character)
}
// 依序印出
// D
// o
// g
// !

```

後面章節會正式介紹 [for-in 的使用方法](../ch1/control_flow.md#for)。

<a name="concatenating_strings_characters"></a>
### 連接字元及字串

可以簡單的使用加號`+`將兩個字串連結在一起，加號指派運算`+=`同樣也可以使用。字元也是一樣的使用方式。

```swift
let str = "Hello"
let secondStr = ", world ."
var anotherStr = str + secondStr
// 印出：Hello, world .
print(anotherStr)

anotherStr += " Have a nice day ."
// 印出：Hello, world . Have a nice day .
print(anotherStr)

```

<a name="string_interpolation"></a>
### 字串插值

可以使用反斜線`\`接著小括號`()`：`\(變數、常數或表達式)`來將其內的值插入到一個字串中。

```swift
let str = "Sunday"
var anotherStr = "It is \(str) ."
// 印出：It is Sunday .
print(anotherStr)

// 表達式也可以
// 印出：I have 13 cars .
print("I have \(1 + 2 * 6) cars .")

```

<a name="special_characters"></a>
### 特殊符號

字串中可以使用下面這些特殊符號：

- 跳脫字元：`\0`(空字元)、`\\`(反斜線)、`\t`(水平 tab)、`\n`(換行)、`\r`(回車)、`\"`(雙引號)、`\'`(單引號)。
- Unicode 純量：寫成\u{n}(u為小寫)，其中 n 為任意一到八位十六進制數且可用的 Unicode 位碼。

```swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein" // 印出 "Imagination is more important than knowledge" - Einstein
let dollarSign = "\u{24}"        // $,  Unicode 純量 U+0024
let blackHeart = "\u{2665}"      // ♥,  Unicode 純量 U+2665

```

<a name="counting_characters"></a>
### 計算字串中的字元數量

```swift
let str = "What a lovely day !"

// 印出字元數量：19
print(str.characters.count)

```

<a name="comparing_strings"></a>
### 比較字串

有三種方式來比較字串：

- 字串相同或不同 `==`、`!=`
- 前綴相同 `hasPrefix`
- 後綴相同 `hasSuffix`

```swift
let str = "It is Sunday ."
let str2 = "It is Sunday ."
let str3 = "It is Saturday ."

// 兩個字串相同 所以成立
if str == str2 {
    print("Success")
}
// 印出：Success

// str2 有前綴字串 It is 所以成立
if str2.hasPrefix("It is") {
    print("Success")
}
// 印出：Success

// str3 沒有後綴字串 Sunday . 所以不成立
if str3.hasSuffix("Sunday .") {
    print("Success")
} else {
    print("Failure")
}
// 印出：Failure


```

可以看到有`str.characters`、`str.characters.count`或是`str.hasPrefix()`這種以小數點`.`連接的表示方式，代表的是這個變數的屬性或是方法。

使用方法會依照其設定的規則表示，像是`str.characters`就是這個字串的字元集合，`str.characters.count`是字元集合的字元數量。而`str.hasPrefix()`則是會對變數作處理後再返回。往後會很常見到這種用法。

