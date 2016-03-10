# 字串及字元

字元指的是依照編碼格式的一個單位元組，而字串是有序的字元集合(簡單說就是一段文字)，皆是以兩個雙引號`"`前後包起來。

#### 字串字面量 String Literals

你可以在你的程式碼中包含一段預先定義的字串值作為字串字面量。字串字面量是由雙引號`""`包著的具有固定順序的文字字元集，可以用於為常數和變數提供初始值。

```swift
let someString = "Some string literal value"

```

#### 初始化空字串

將空的字串字面量指派給變數，也可以初始化一個新的`String`實體：

```swift
var emptyString = ""
var anotherEmptyString = String()
// 這兩個是一樣的意思

```

#### 使用字元

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

後面章節會正式介紹`for-in`的使用方法。

#### 計算字串中的字元數量

```swift
let str = "What a lovely day !"
print(str.characters.count) // 會印出字元數量 19

```

#### 連接字元及字串

可以簡單的使用加號`+`將兩個字串連結在一起，加號賦值運算`+=`同樣也可以使用。字元也是一樣的使用方式。

```swift
let str = "Hello"
let secondStr = ", world ."
var anotherStr = str + secondStr
print(anotherStr) // 印出 Hello, world .

anotherStr += " Have a nice day ."
print(anotherStr) // 印出 Hello, world . Have a nice day .

```

#### 字串插值

可以使用反斜線`\`接著小括號`()`：`\(變數、常數或表達式)`來將其內的值插入到一個字串中。

```swift
let str = "Sunday"
var anotherStr = "It is \(str) ."
print(anotherStr) // 印出 It is Sunday .

// 表達式也可以
print("I have \(1 + 2 * 6) cars .") // 印出 I have 13 cars .

```

#### 特殊符號

字串中可以使用下面這些特殊符號：

- 跳脫字元：`\0`(空字元)、`\\`(反斜線)、`\t`(水平 tab)、`\n`(換行)、`\r`(回車)、`\"`(雙引號)、`\'`(單引號)。
- Unicode 純量：寫成\u{n}(u為小寫)，其中 n 為任意一到八位十六進制數且可用的 Unicode 位碼。

```swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein" // 印出 "Imagination is more important than knowledge" - Einstein
let dollarSign = "\u{24}"        // $,  Unicode 純量 U+0024
let blackHeart = "\u{2665}"      // ♥,  Unicode 純量 U+2665

```

#### 比較字串

有三種方式來比較字串

- 字串相同 `==`
- 前綴相同 `hasPrefix`
- 後綴相同 `hasSuffix`

```swift
let str = "It is Sunday ."
let str2 = "It is Sunday ."
let str3 = "It is Saturday ."

// 兩個字串相同 所以成立 印出 Success
if str == str2 {
    print("Success")
}

// str2 有前綴字串 It is 所以成立 印出 Success
if str2.hasPrefix("It is") {
    print("Success")
}

// str3 沒有後綴字串 Sunday . 所以不成立 印出 Failure
if str3.hasSuffix("Sunday .") {
    print("Success")
} else {
    print("Failure")
}

```

##### Hint

可以看到有`str.characters`、`str.characters.count`或是`str.hasPrefix()`這種以小數點`.`連接的表示方式，代表的是這個變數的屬性或是方法。

使用方法會依照其設定的規則表示，像是`str.characters`就是這個字串的字元集合，`str.characters.count`是字元集合的字元數量。而`str.hasPrefix()`則是會對變數作處理後再返回。往後會很常見到這種用法。






