# 自動參考計數

Swift 使用自動參考計數(`ARC`, `Automatic Reference Counting`)機制來追蹤與管理記憶體使用狀況，所以大部分情況下，你不需要自己管理，Swift 會自動釋放掉不需要的記憶體。

##### Hint：參考計數只應用在類別的實體。結構與列舉為值型別，也不是通過參考的方式儲存與傳遞。

當一個類別實體被指派值(給一個屬性、常數或變數)的時候，會建立一個該實體的**強參考**(`strong reference`)，同時會將**參考計數**(`reference counting`)加 1 ，強參考表示會將這個實體保留住，只要強參考還在(也就是參考計數不為 0 )，儲存這個實體的記憶體就不會被釋放掉。

以下簡單介紹一下 **ARC** 運作的方式：

```swift
// 定義一個類別 Person
class Person {
    let name: String
    init (name: String) {
        self.name = name
    }
}

// 先宣告三個可選 Person 的變數 會被自動初始化為 nil
// 這三個變數目前都尚未有實體的參考
var reference1: Person?
var reference2: Person?
var reference3: Person?

// 先生成一個實體 並指派給其中一個變數 reference1
reference1 = Person(name: "Jess")

// 目前這個實體就有了一個強參考 參考計數為 1
// 所以 ARC 會保留住這個實體使用的記憶體

// 接著再指派給另外兩個變數
reference2 = reference1
reference3 = reference1

// 這時這個實體多了 2 個強參考 總共為 3 個強參考
// 也就是目前的參考計數為 3

// 接著將其中兩個變數指派為 nil 斷開他們的強參考
reference1 = nil
reference2 = nil

// 目前還有 1 個強參考 參考計數為 1
// 所以 ARC 仍然會保留住記憶體

// 最後將第三個變數也指派為 nil 斷開強參考
reference3 = nil

// 這時這個實體已經沒有強參考了 參考計數為 0
// ARC 就會將記憶體釋放掉

```


### 類別實體間的強參考循環


### 解決實體間的強參考循環

#### 弱參考

#### 無主參考

#### 無主參考和隱式解析可選型別


### 閉包的強參考循環


### 解決閉包的強參考循環

#### 定義捕獲列表

#### 弱參考和無主參考






