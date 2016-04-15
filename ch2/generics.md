# 泛型

泛型(`generic`)是 Swift 一個重要的特性，可以讓你自定義出一個**適用任意型別**的函式及型別。可以避免重複的程式碼且清楚的表達程式碼的目的。

許多 Swift 標準函式庫就是經由泛型程式碼建構出來的，像是陣列(`Array`)和字典(`Dictionary`)都是泛型的，你可以宣告一個`[Int]`陣列，也可以宣告一個`[String]`陣列。同樣地，你也可以宣告任意指定型別的字典。

你可以將泛型使用在函式、列舉、結構及類別上。


### 泛型能解決的問題

以下是一個可以利用泛型來簡化程式碼的例子：

```swift
// 定義一個將兩個整數變數的值互換的函式
func swapTwoInts(inout a: Int, inout _ b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}

// 定義兩個整數變數 並當做參數傳入函式
var oneInt = 12
var anotherInt = 500
swapTwoInts(&oneInt, &anotherInt)

print("互換後的 oneInt 為 \(oneInt)，anotherInt 為 \(anotherInt)")
// 印出：互換後的 oneInt 為 500，anotherInt 為 12

// 與上面定義的函式功能相同 只是這時互換的變數型別為字串
func swapTwoStrings(inout a: String, inout _ b: String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

```

由上述程式可以看到，兩個函式的功能完全一樣，唯一不同的只有傳入參數的型別，這種情況便可以使用泛型來簡化。


### 泛型函式

根據前面提到的兩個功能完全一樣的函式，以下使用泛型來定義一個**適用任意型別**的函式：

```swift
func swapTwoValues<T>(inout a: T, inout _ b: T) {
    let temporaryA = a
    a = b
    b = temporaryA
}

```

上述程式中的函式使用了**佔位型別名稱**(`placeholder type name`，習慣以字母`T`來表示)來代替實際型別名稱(像是`Int`、`Double`或`String`)。

可以注意到函式名稱後面緊接著一組**角括號`<>`**，且將`T`包含著。這代表角括號內的`T`是函式定義的一個佔位型別名稱，因此 Swift 不會去查找名稱為`T`的實際型別。

定義佔位型別名稱時不會明確表示`T`是什麼型別，但參數`a`與`b`都必須是這個`T`型別。而只有當這個函式被呼叫時，才會根據傳入參數的實際型別，來決定`T`所代表的型別。

這時便可以使用這個泛型函式，如下：

```swift
//  首先是兩個整數
var oneInt = 12
var anotherInt = 320
swapTwoValues(&oneInt, &anotherInt)

// 再來是兩個字串
var oneString = "Hello"
var anotherString = "world"
swapTwoValues(&oneString, &anotherString)

```


### 型別參數

前面提到的`swapTwoValues(_:_:)`中，佔位型別名稱`T`是型別參數的一個例子。

**型別參數**會指定並命名一個佔位型別，且會緊跟在函式名稱後面使用一組角括號`<>`包起來。當一個型別參數被指定後，就可以用來定義一個**函式的參數型別**、**函式的返回值型別**或是**函式內的型別標註**。

型別參數可以指定一個或一個以上，使用多個時以逗號`,`隔開。

#### 命名型別參數

在一般情況下，型別參數會指定為一個有描述性的名字，像是`Dictionary<Key, Value>`中的`Key`和`Value`，或是`Array<Element>`中的`Element`，用來明顯表示這些型別參數與泛型函式之間的關係。而當無法有意義的描述型別參數時，通常會使用單一字母來命名，像是`T`、`U`或`V`。

##### Hint

- 通常會使用大駝峰命名法(像是`T`或`MyTypeParameter`)來為型別參數命名，以表示他們是佔位型別，而不是一個值。


### 泛型型別

除了泛型函式，你也可以定義一個**泛型型別**。你可以定義在列舉、結構或類別上，類似陣列(`Array`)和字典(`Dictionary`)。

以下會定義一個**堆疊(`Stack`)**的泛型集合型別來當做一個例子。堆疊的運作方式有點像陣列，可以增加(`push`)一個元素到陣列最後一員，也可以從陣列中取出(`pop`)最後一個元素。

```swift
// 定義一個泛型結構 Stack 其佔位型別參數命名為 Element
struct Stack<Element> {
    // 將型別參數用於型別標註 設置一個型別為 [Element] 的空陣列
    var items = [Element]()
    
    // 型別參數用於方法的參數型別 方法功能是增加一個元素到陣列最後一員
    mutating func push(item: Element) {
        items.append(item)
    }

    // 型別參數用於方法的返回值型別 方法功能是移除陣列的最後一個元素
    mutating func pop() -> Element {
        return items.removeLast()
    }
}

```

上述定義的結構中可以看到，指定`Element`為佔位型別參數後，便可在結構中作為**型別標註**、**方法的參數型別**及**方法的返回值型別**，而因為必須修改結構的內容，所以方法都必須加上`mutating`。

接著就可以使用這個剛定義好的`Stack`型別，如下：

```swift
// 先宣告一個空的 Stack
var stackOfStrings = Stack<String>()

// 依序放入三個字串
stackOfStrings.push("one")
stackOfStrings.push("two")
stackOfStrings.push("three")

// 然後移除掉最後一個元素 即字串 "three"
stackOfStrings.pop()

// 現在這個 Stack 還有兩個元素 分別為 one 及 two

```

#### 擴展一個泛型型別

當你擴展一個泛型型別時，不需要在擴展的定義中提供型別參數列表，原型別已經定義的型別參數列表(如前面提到的`Stack`定義的`Element`)可以直接在擴展中使用。

以下為堆疊(`Stack`)擴展一個名稱為`topItem`的唯讀計算屬性，它會返回這個堆疊的最後一個元素，且不會將其移除：

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}

```

上述程式可以看到，擴展中可以直接使用`Element`。而返回值為一個可選值，所以以下使用可選綁定來取得最後一個元素：

```swift
if let topItem = stackOfStrings.topItem {
    print("最後一個元素為 \(topItem)")
    // 印出：最後一個元素為 two
}

```


### 型別約束

有時在定義一個泛型函式或泛型型別時，會需要為這個泛型型別參數增加一些限制，可能是指定型別參數必須繼承自指定的類別，或是符合一個特定的協定。

像是 Swift 內建的字典(`Dictionary`)便對字典的鍵的型別作了些限制。字典的鍵的型別必須是**可雜湊的(`hashable`)**，也就是必須只有唯一一種方式可以表示這個鍵。

而實際上為了實現這個限制，字典的鍵的型別符合了`Hashable`協定。**`Hashable`**是 Swift 標準函式庫中定義的一個特定協定，所有 Swift 的基本型別(像是`Int`、`Double`、`Bool`和`String`)預設都是可雜湊的(`hashable`)。

#### 型別約束語法

你可以在一個型別參數名稱後面加上冒號`:`並緊接著一個類別或是協定來做為型別約束，它們會成為型別參數列表的一部分，例子如下(泛型型別也是一樣方式)：

```swift
func 泛型函式名稱<T: 某個類別, U: 某個協定>(參數: T, 另一個參數: U) {
    函式內部的程式
}

```

上述定義中可以看到，`T`型別參數必須繼承自**某個類別**，`U`型別參數則必須遵循**某個協定**。

#### 使用型別約束

以下會定義一個函式，兩個參數分別為一個陣列及一個值，函式的功能是尋找第一個參數陣列中是否有另一個參數值，如果有就返回這個值在陣列中的索引值，找不到則返回`nil`。

這個函式的型別約束會使用到另一個 Swift 標準函式庫中的`Equatable`協定，這個協定要求任何遵循該協定的型別必須實作`==`及`!=`，進而可以對該型別的任意兩個值進行比較。(所有的 Swift 標準型別預設都符合`Equatable`協定。)

```swift
func findIndex<T: Equatable>(array: [T], _ valueToFind: T) -> Int? {
    for (index, value) in array.enumerate() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}

// 首先找看看 [Double] 陣列的值
let doubleIndex = findIndex([689, 5566, 10.05], 9.2)
// 因為 9.2 不在陣列中 所以返回 nil

// 接著找 [String] 陣列的值
let stringIndex = findIndex(["Adam", "Kevin", "Jess"], "Kevin")
// Kevin 為陣列中第 2 個值 所以會返回 1

```


### 關聯型別

#### 使用關聯型別

#### 經由擴展一個已存在的型別來設置關聯型別


### Where 語句





