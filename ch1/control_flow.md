# 控制流程

Swift 提供了可以循環執行任務的`for`和`while`迴圈，以及根據條件選擇執行不同程式碼分支的`if`和`switch`語句，還有控制流程跳轉到其他程式碼的`break`和`continue`語句。

### For 循環

Swift 提供兩種`for`循環方式，按照指定的次數執行任務。

- `for-in` 循環對一個集合(像是陣列`Array`、字典`Dictionary`)遍歷其內所有的元素，操作此元素並執行循環內的程式。
- `for` 循環會重複執行內部的程式，直到設定的條件達成，通常會在每次循環完成後增加計數器的值來實現。

#### For-in

使用`for-in`遍歷一個集合內的所有元素，像是一個數字區間、陣列中的值或是字串內的字元。

```swift
// 一個一到五的閉區間
for index in 1...3 {
    print(index)
}
// 會依序印出
// 1
// 2
// 3
```

上述程式內的`index`不用做宣告的動作，他在每次遍歷開始時會被自動指派值。

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

###for

```
//原始用法
var n = 0
for var i = 0; i < 5 ; i++ {
    n += i
}

// 與上面相同 不包含上限
for i in 0..<5 {
    n += i
}

// 包含上限
for i in 0...5 {
    n += i
}
```


### if
```
if var v = vvv {
}

var n = "hi, \(n1 ?? n2)"
```

### switch
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



###while and repeat-while
```
var n = 2
while n < 20 {
    n = n * 2
}

// 一定會先執行一次再看 while 條件
repeat {
    n = n * 2
} while n < 100

```


