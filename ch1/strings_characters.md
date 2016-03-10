# 字串及字元

字元指的是依照編碼格式的一個單位元組，而字串是有序的字元集合(簡單說就是一段文字)，皆是以兩個雙引號`"`前後包起來。

#### 初始化空字串

可以用下面這兩種方式初始化空字串。

```swift
var emptyString = ""
var anotherEmptyString = String()
// 這兩個是一樣的意思

```

#### 使用字元

前面提到，字串是有序的字元集合，所以可以使用`for-in`迴圈來遍歷字串中的每一個字元：

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

可以簡單的使用加號`+`將兩個字串連結在一起，複合


