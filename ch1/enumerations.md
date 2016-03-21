# 列舉 Enumerations

列舉(`enumeration`)是可以讓你自定義一個型別的一組相關的值，使你可以在程式碼中以型別安全(`type-safe`)的方式來使用這些值。

### 列舉語法

列舉使用`enum`關鍵字建立，並將列舉定義放在一組大括號`{}`內，格式如下：

```swift
enum 列舉的自定義型別 {
    各列舉定義
}

```

列舉使用`case`關鍵字定義成員值，例子如下：

```swift
//這是一個定義指南針四個方位的列舉
enum CompassPoint {
    case North
    case South
    case East
    case West
}

// 多個成員值可以寫在同一行 以逗號 , 隔開
// 這是一個定義太陽系八大行星的列舉
enum Planet {
    case Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}

```

每個列舉都定義了一個全新的型別。像 Swift 中其他型別一樣，列舉的名稱(如上述程式中的`CompassPoint`及`Planet`)應該以一個大寫字母開頭且為單數。

定義完列舉後，接著將其指派給一個變數，與一般指派方式相同：(這邊使用上面定義過的指南針方位的列舉)

```swift
// 型別為 CompassPoint 的一個變數 值為其列舉內的 West 
var directionToHead = CompassPoint.West

// 這時已經可以自動推斷這個變數的型別為 CompassPoint
// 如果要再指派新的值 可以省略列舉的型別名稱
directionToHead = .North

```


### 使用 Switch 語句匹配列舉值

這邊同樣使用上面定義過的指南針方位的列舉。使用`switch`語句匹配單個列舉值：

```swift
directionToHead = .South
switch directionToHead {
    case .North:
        print("Lots of planets have a north")
    case .South:
        print("Watch out for penguins") // 這行會被印出
    case .East:
        print("Where the sun rises")
    case .West:
        print("Where the skies are blue")
}

```


### 相關值 Associated Values

列舉中的每個成員值，視需求可以在需要的時候，一併儲存自定義的一個或以上**其他型別**的**相關值**(`associated values`)。使用方法為在成員值後面加上小括號`()`，並將**相關值型別**放在小括號內(就像使用元組`tuple`一樣)。往後在程式中將該列舉成員值指派給變數或常數時，這個(或這些)相關值才會被設置，且可以是不同的。

以下是一個例子，假設建立一個庫存追蹤系統，商品條碼可能會有`UPC-A`格式的一維碼，每一個`UPC-A`條碼是一組四個正整數的值，或是使用`QR Code`格式的二維碼，每一個`QR Code`條碼是一個最多為 2,953 字元的字串，依據這個條件建立的列舉如下：

```swift
enum Barcode {
    case UPCA(Int, Int, Int, Int)
    case QRCode(String)
}

```

上述程式的意思是：定義一個名稱為`Barcode`的列舉型別，有兩個成員值，一個成員值為`UPCA`，夾帶著`(Int, Int, Int, Int)`型別的相關值，另一個成員值為`QRCode`，夾帶著`String`型別的相關值。

這個定義沒有提供任何`Int`或`String`的實際的值，它只是定義了：當一個型別為`Barcode`的變數或常數等於`Barcode.UPCA`或`Barcode.QRCode`時，可以儲存的相關值的型別。

這時可以指派一個型別為`Barcode`的變數，例子如下：

```swift
// 指派 Barcode 型別 成員值為 UPCA
// 相關值為 (8, 85909, 51226, 3)
var productBarcode = Barcode.UPCA(8, 85909, 51226, 3)

// 如果要修改為儲存 QR Code 條碼
productBarcode = .QRCode("ABCDEFG")

// 這時 .UPCA(8, 85909, 51226, 3) 會被 .QRCode("ABCDEFG") 所取代
// 一個變數 同一時間只能儲存一個列舉的成員值(及其相關值)

```

在使用`switch`語句匹配列舉值時，可以把相關值取出作為常數(`let`)或變數(`var`)使用，例子如下：

```swift
switch productBarcode {
case .UPCA(let numberSystem, let manufacturer, let product, let check):
    print("UPC-A: \(numberSystem), \(manufacturer), \(product), \(check).")
case .QRCode(let productCode):
    print("QR Code: \(productCode).") // 會印出這行
}

```

如果相關值同樣都被取出作為常數(或變數)，可以改成只在成員名稱前加上`let`(或`var`)，來使程式更為簡潔，如下：

```swift
switch productBarcode {
case let .UPCA(numberSystem, manufacturer, product, check):
    print("UPC-A: \(numberSystem), \(manufacturer), \(product), \(check).")
case let .QRCode(productCode):
    print("QR Code: \(productCode).")
}

```


### 原始值 Raw Values

除了使用相關值的列舉，其內的成員值可以儲存不同型別的相關值。Swift 也提供列舉先設置原始值(`raw value`)來代替相關值，這些原始值的**型別必須相同**。使用方法為在列舉名稱後加上冒號`:`並接著**原始值型別**，例子如下：

```swift
enum WeekDay: Int {
    case Monday = 1
    case Tuesday = 2
    case Wednesday = 3
    case Thursday = 4
    case Friday = 5
    case Saturday = 6
    case Sunday = 7
}

let today = WeekDay.Friday
// 使用 rawValue 屬性來取得原始值
print(today.rawValue) // 印出 5

```

##### Hint

- 原始值可以是字串、字元或者任何整數值或浮點數值。
- 每個原始值在它的列舉宣告中必須是唯一的。
- 當整數值被用於原始值，如果其他列舉成員沒有值時，它們會自動遞增。

原始值(`raw value`)跟相關值(`associated value`)是不同的。原始值在定義列舉時即被設置，對於一個特定的列舉成員，它的原始值始終是相同的。而相關值是在列舉成員被指派為一個變數(或常數)時才一併設置的值，列舉成員的相關值是可以不同的。


#### 原始值的隱式指派 Implicitly Assigned Raw Values

在使用原始值為**整數**型別的列舉時，可以不需要為每個成員設置原始值，Swift 會將每個成員的原始值依次遞增`1`。成員都沒有原始值時，則會將第一個成員的原始值設置為`0`，再依序遞增`1`，例子如下：

```swift
// 第一個成員有設置原始值 1, 接著下去成員的原始值就是 2, 3, 4 這樣遞增下去
enum Planet: Int {
    case Mercury = 1, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}

let ourPlanet = Planet.Earth
print(ourPlanet.rawValue) // 印出 3

```

在使用原始值為**字串**型別的列舉時，可以不需要為每個成員設置原始值，將會直接將該成員值設置為原始值。例子如下：

```swift
enum CompassPoint: String {
    case North, South, East, West
}

let directionPoint = CompassPoint.East
print(directionPoint.rawValue) // 印出 East

```


### 使用原始值初始化列舉實體 Initializing from a Raw Value

在定義列舉時，如果使用了原始值，則這個列舉會有一個初始化方法(`method`)，這個方法有一個名稱為`rawValue`的參數，其參數型別就是列舉原始值的型別，返回值為列舉成員或`nil`。例子如下：

```swift
// 一個使用原始值的列舉 原始值依序是 1,2,3,4,5,6,7,8
enum Planet: Int {
    case Mercury = 1, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}

let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet 型別為 Planet? 值為 Planet.Uranus

```

不過不是所有傳入的`Int`參數都會有返回值，所以其實他是返回一個`Planet?`型別，也就是可選的`Planet`。以下是一個返回`nil`的例子：

```swift
let positionToFind = 9
if let targetPlanet = Planet(rawValue: positionToFind) {
    switch targetPlanet {
    case .Earth:
        print("We are here !")
    default:
        print("Not Safe !")
    }
} else {
    print("No planet at position \(positionToFind)")
}
// 印出 No planet at position 9

```

上述程式先使用了一個可選綁定(`optional binding`)，使用原始值`9`來尋找是否有星球，但可以看到列舉`Planet`中沒有原始值為`9`的成員，所以會返回一個`nil`，接著則執行`else`內部程式。


### 遞迴列舉 Recursive Enumerations






