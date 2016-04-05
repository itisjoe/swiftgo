# 錯誤處理

程式運行中，有時會遇到錯誤需要處理，像是需要讀取一個檔案，但檔案可能不存在或是沒有讀取權限，還有像是一個購物車需要進行業務邏輯上的判斷，結帳前要檢查是否有商品或是超過數量限制等等。對此 Swift 提供了完整的對於錯誤的拋出、捕獲、傳遞及處理的支持。


### 錯誤的描述與拋出

首先我們必須定義一組錯誤描述，來讓程式中遇到錯誤時，可以清楚知道當前是遇到了什麼錯誤，以及各自匹配後續的處理。Swift 中通常是使用一個遵循`ErrorType`協定(`protocol`)的列舉來表示一組錯誤描述(`ErrorType`是一個空的協定，只是為了告訴 Swift 這個列舉是用來表示錯誤描述)。

下面例子為一組自動販賣機錯誤描述的列舉，成員依序為：**無此商品**、**金額不足**(有一個參數為還需要補足多少錢幣)及**商品已賣光**：

```swift
enum VendingMachineError: ErrorType {
    case InvalidSelection
    case InsufficientFunds(coinsNeeded: Int)
    case OutOfStock
}

```

當遇到一個錯誤的時候，我們會表示這邊有一個錯誤發生，然後會將這個錯誤**拋出**，後續再由自己定義處理方式或交由 Swift 自動處理。Swift 中使用`throw`語句來拋出一個錯誤。下面例子表示拋出一個**自動販賣機還需要補足 3 個錢幣**的錯誤：

```swift
throw VendingMachineError.InsufficientFunds(coinsNeeded: 3)

```


### 使用拋出函式傳遞錯誤

前面說明了如何拋出錯誤，接著我們必須設計一個函式，其內部會經過邏輯判斷是否發生異常或錯誤，當發生時便會拋出錯誤。Swift 使用`throws`關鍵字來標記函式，稱其為**拋出函式**(`throwing function`)，格式如下：

```swift
func 函式名稱() throws -> 返回值型別 {
    內部執行的程式
}

```

##### Hint

- 拋出函式中的拋出(`throw`)有點類似`return`，因為拋出一個錯誤表示一個異常或錯誤發生了，所以正常的執行流程會立即中止，其後的程式都不會繼續執行，會直接傳遞至處理錯誤的地方繼續。
- 只有拋出函式可以傳遞錯誤。任何在一個非拋出函式中拋出的錯誤都必須在該函式內部處理。

以下定義一個自動販賣機的類別：

```swift
// 先定義一個結構來表示一個商品的內容 分別為商品的價錢及數量
struct Item {
    var price: Int
    var count: Int
}

// 定義一個自動販賣機的類別
class VendingMachine {
    // 自動販賣機內的商品
    var inventory = [
        "可樂": Item(price: 25, count: 4),
        "洋芋片": Item(price: 20, count: 7),
        "巧克力": Item(price: 35, count: 11)
    ]

    // 目前已投入了多少錢幣 預設值為 0
    var coinsDeposited = 0
    
    // 所有判斷錯誤的邏輯都通過後 確定購買商品的動作
    func dispenseSnack(snack: String) {
        print("Dispensing \(snack)")
    }

    // 販售的動作 確定售出前會做些判斷
    // 這是一個拋出函式 所以函式名稱需要加上 throws
    func vend(itemNamed name: String) throws {
        // 檢查是否有這個商品 沒有的話會拋出錯誤
        guard var item = inventory[name] else {
            throw VendingMachineError.InvalidSelection
        }

        // 檢查這個商品是否還有剩 已賣光的話會拋出錯誤
        guard item.count > 0 else {
            throw VendingMachineError.OutOfStock
        }

        // 檢查目前投入的錢幣夠不夠 不夠的話會拋出錯誤
        guard item.price <= coinsDeposited else {
            // 參數為還需要補足多少錢幣 所以是商品價錢減掉已投入錢幣
            throw VendingMachineError.InsufficientFunds(coinsNeeded: item.price - coinsDeposited)
        }

        // 所有判斷都通過後 才確定會售出
        coinsDeposited -= item.price
        item.count -= 1
        inventory[name] = item
        dispenseSnack(name)
    }
}

```


### 錯誤的捕獲及處理

前面定義的類別中有一個**拋出函式**，如果遇到錯誤時會將錯誤拋出並傳遞至錯誤處理的地方，目前尚未定義怎麼處理錯誤，所以這時 Swift 會自動處理，不過這可能會導致程式中止，所以我們還是自行定義錯誤處理的方式。

Swift 使用`do-catch`語句來定義錯誤的捕獲(`catch`)及處理，每一個`catch`表示可以捕獲到一個錯誤拋出的處理方式，以下是格式：

```swift
do {
    try 拋出函式
    其他執行的程式
} catch 錯誤1 {
    處理錯誤1
} catch 錯誤2 {
    處理錯誤2
}

```

##### Hint

- 如果要呼叫**拋出函式**，必須在函式前加上`try`關鍵字。
- 如果要捕獲拋出的錯誤，必須將拋出函式(或拋出錯誤的程式)寫在關鍵字`do`包含的大括號`{}`內。
- 使用關鍵字`catch`來匹配要捕獲的每個錯誤。

底下是一個例子：

```swift
// 生成一個自動販賣機類別的實體 並設置已投入 8 個錢幣
var vendingMachine = VendingMachine()
vendingMachine.coinsDeposited = 8

// 進行錯誤的拋出、捕獲及處理
do {
    // 呼叫拋出函式 我要購買可樂這個商品
    try vendingMachine.vend(itemNamed: "可樂")

    // 其他可能需要執行的程式 這邊先省略

// 以下每個 catch 為各自匹配錯誤的處理
} catch VendingMachineError.InvalidSelection {
    print("無此商品")
} catch VendingMachineError.OutOfStock {
    print("商品已賣光")
} catch VendingMachineError.InsufficientFunds(let coinsNeeded) {
    print("金額不足，還差 \(coinsNeeded) 個錢幣")
}

```

上述程式中可以看到`vendingMachine.vend(itemNamed:)`函式內，每個會拋出的錯誤都可以在`do-catch`中列出的`catch`語句中匹配到。

而上述這個例子依序判斷錯誤到最後，會因為錢幣不足而拋出`VendingMachineError.InsufficientFunds`這個錯誤，並在外面的`do-catch`中被捕獲到，最後會印出：**金額不足，還差 17 個錢幣**。當然因為已經拋出錯誤，其後的程式都不會繼續執行。


### 轉換錯誤為可選值

前面提到可以使用一個**拋出函式**來拋出錯誤，並使用`do-catch`來捕獲並處理錯誤。而如果只是要簡單讓錯誤發生時，返回為一個`nil`，像是如下的表示：

```swift
// 定義一個拋出函式 會返回一個 Int
func someThrowingFunction() throws -> Int {
    // 內部執行的程式
}

// 宣告一個可選型別 Int? 的常數 x 
let x: Int?
do {
    // 呼叫拋出函式 會返回一個 Int
    x = try someThrowingFunction()
} catch {
    // 錯誤發生而被拋出 進而捕獲時 將其設為 nil
    x = nil
}

```

如上述程式的功能，我們可以簡單的將`try`改成使用`try?`，這樣當錯誤發生要被拋出時，會簡單的返回一個`nil`。如下：

```swift
let x = try? someThrowingFunction()

```

##### Hint：不論原本拋出函式返回的是不是可選值，使用`try?`呼叫的拋出函式，都會返回可選值。


### 禁用錯誤傳遞

當你知道一個拋出函式確定不會在執行時拋出錯誤，這時可以使用`try!`來呼叫拋出函式，這樣會將錯誤傳遞禁用，但當錯誤真的被拋出時，會發生程式運行時錯誤。

也就是說，使用`try!`呼叫拋出函式來告訴 Swift 確定這個呼叫不會發生異常或錯誤。還有一點，使用`try!`呼叫拋出函式，可以不用放在`do`的大括號`{}`內。例子如下：

```swift
let x = try! someThrowingFunction()

```


### 必定執行的程式區塊

我們可以使用`defer`定義一個程式區塊，當在無論是拋出(`throw`)錯誤，或是使用`return`、`break`結束這個函式後，都必定會執行這個程式區塊。

當在需要做清理工作或是釋放記憶體之類的程式時很好用，像是一個開啟檔案的函式，如下：

```swift
func someMethod() throws {
    // 打開一個資源 像是開啟一個檔案
	
    defer {
        // 釋放資源記憶體或清理工作
        // 像是關閉一個開啟的檔案
    }
	
    // 錯誤處理 像是檔案不存在或沒有讀取權限

    // 及其他要執行的程式
}

```

依照上述程式，這樣不論在正常執行程式到最後或是因為發生錯誤拋出而中止，最後都會執行`defer`內的程式，保證清理工作一定會執行。

##### Hint

- 如果定義多個`defer`，會先執行最後一個定義的`defer`，再依序往前執行到第一個。
- `defer`不是一定要與錯誤處理一起使用，普通的函式內也可以使用。

