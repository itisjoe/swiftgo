# 集合型別

Swift 提供三種基本的集合型別：`Arrays`、`Sets`、`Dictionaries`來儲存集合資料。儲存的資料**型別必須明確**，且都**只能儲存同一種型別**的資料。

- Array 陣列：按順序儲存資料。
- Set 集合：沒有順序、也不能重複儲存資料。
- Dictionary 字典：沒有順序，鍵值對 `key : value` ，也就是可以經由唯一的識別鍵找到需要的值。


### Arrays 陣列

陣列使用有序列表儲存同一型別的多個值。相同的值可以多次出現在一個陣列的不同位置中。

宣告陣列變數或常數時的型別，有`Array<Element>`及`[Element]`兩種方式(`Element`是需要明確表示的型別，如`Int`、`String`、`Double`等等)。

```swift
// 宣告儲存 Int 型別的陣列
var arr: Array<Int>
var arr2: [Int]
// 兩種陣列型別表示方式 在功能上是一樣的 所以用第二種就好

```

#### 創建一個空陣列

```swift
// 宣告一個型別為 Int 的空陣列
var arr = [Int]()

// 為這個陣列加上一個值
arr.append(12)

// 這時如果又要再將這個陣列指派成空陣列
// 因為前面宣告時已經定義好型別
// 所以可以很簡單的使用 [] 來指派成空陣列
arr = []

// 或是首次宣告變數時 有明確定義好型別 也可以使用 []
var anotherArr: [Int] = []

```

#### 創建一個帶有預設值的陣列

陣列型別還提供一個可以創建特定大小並且所有資料都被預設的建構方法。將準備加入新陣列的資料項數量（count）和適當型別的初始值（repeatedValue）傳入陣列建構函式：

```swift
var threeInts = [Int](count: 3, repeatedValue:0)
// threeInts 是一種 [Int] 陣列, 等於 [0, 0, 0]

```

#### 合併兩個陣列

陣列可以使用加法運算子`+`來合併兩個陣列。

```swift
// 首先創建一個 [0,0,0] 的陣列
var threeInts = [Int](count: 3, repeatedValue:0)

// 接著再創建一個 [2,2,2] 的陣列
var anotherThreeInts = [Int](count: 3, repeatedValue:2)

// 最後將兩個陣列合併
var SixInts = threeInts + anotherThreeInts
// 會變成 [0,0,0,2,2,2]

```

#### 直接填入值來創建陣列

可以直接填入值來創建陣列，每個值以逗號`,`分隔。

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// 即創建了一個型別為 String 包含兩個值的陣列

// 因為 Swift 會自動的型別推斷 所以陣列中如果明確的表示了是什麼型別的值 便不用再

```



### Sets 集合




### Dictionaries 字典