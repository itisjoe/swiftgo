# 可選鏈

- [以可選鏈替代強制解析](#optional_chaining_as_an_alternative_to_forced_unwrapping)
- [通過可選鏈存取屬性、下標及呼叫方法](#accessing_properties_subscripts_methods_through_optional_chaining)

可選鏈(`optional chaining`)是一個可以存取或呼叫屬性(`property`)、方法(`method`)及下標(`subscript`)的過程。

稱其為**可選**(`optional`)是因為當前存取或呼叫的目標可能為空(`nil`)，而多次存取或呼叫可以用**點語法**(`dot syntax` 即`.`)將其全部鏈結在一起，所以稱為鏈(`chaining`)。

可選鏈中，只要其中一個節點為空(`nil`)，則會立即返回`nil`。相對地，如果存取或呼叫至最後一個節點都有值，則會返回一個**可選型別**的值。

<a name="optional_chaining_as_an_alternative_to_forced_unwrapping"></a>
### 以可選鏈替代強制解析

可選鏈中，如果其中一個節點為可選值時，必須在這個值後面加上問號(`?`)，來表示這是一個可選值，也就是當這個值為空(`nil`)時，會立即返回`nil`。

與強制解析(`forced unwrapping`)在值後面加上驚嘆號(`!`)不同，強制解析的值如果為空(`nil`)時，會觸發程式運行錯誤。而可選鏈中的可選值(加上問號`?`)如果為空(`nil`)，則只會單純的返回`nil`。

不論可選鏈最後返回的屬性、方法或下標是不是可選型別的值，都是返回一個可選值，所以可以用來判斷這個可選鏈是否成功，有返回值是成功，返回`nil`則是失敗。像是原本返回應該是一個`Int`型別的值，經由可選鏈後返回會是一個`Int?`可選型別的值。以下是一個例子：

```swift
// 定義一個類別 Person
class Person {
    // residence 屬性為可選的 Residence 型別
    var residence: Residence?
}

// 定義一個類別 Residence
class Residence {
    // numberOfRooms 屬性為 Int 型別
    var numberOfRooms = 1
}

// 首先生成一個實體
let joe = Person()

// 這時 joe 實體內的可選屬性 residence 沒有設置值 所以會初始化為 nil
// 如果強制解析的話 會發生錯誤 如以下這行
let roomCount = joe.residence!.numberOfRooms

// 所以這時可使用可選鏈來呼叫
if let roomCount = joe.residence?.numberOfRooms {
    print("Joe 有 \(roomCount) 間房間")
} else {
    print("無法取得房間數量")
}

// 上述 if 語句目前會印出：無法取得房間數量
// 接著為 joe.residence 設置一個 Residence 實體
joe.residence = Residence()

// 這時就會返回 1
// 記得這是返回一個可選型別 Int?
print(joe.residence?.numberOfRooms)

```

<a name="accessing_properties_subscripts_methods_through_optional_chaining"></a>
### 通過可選鏈存取屬性、下標及呼叫方法

這邊定義三個類別以供後續示範，沿用先前的類別`Person`，並新增一個類別`Room`，最後將`Residence`重新定義，如下：

```swift
class Person {
    var residence: Residence?
}

// 定義一個類別 Room
class Room {
    // name 屬性為 String 型別
    let name: String

    // 生成實體時同時設置屬性 name 的值
    init(name: String) { self.name = name }
}

// 重新定義類別 Residence
class Residence {
    // rooms 屬性為一個陣列 型別為 [Room] 預設值是空陣列
    var rooms = [Room]()

    // 將 numberOfRooms 改為一個計算屬性 並返回 rooms 的數量
    var numberOfRooms: Int {
        return rooms.count
    }
    
    // 新增一個下標 可以依索引值取得 rooms 陣列內的值
    // 或是依索引值修改 rooms 陣列內的值
    subscript(i: Int) -> Room {
        get {
            return rooms[i]
        }
        set {
            rooms[i] = newValue
        }
    }
    
    // 新增一個方法 簡單的印出房間數量
    func printNumberOfRooms() {
        print("有 \(numberOfRooms) 間房間")
    }

}

```

#### 存取屬性

前面的例子就是示範存取屬性，還要提的一點是，你也可以嘗試使用可選鏈來給屬性指派值，如下：

```swift
// 使用上面生成的實體 joe 設置新的值
joe.residence?.numberOfRooms = 12

```

何以說**嘗試**指派值，因為如果`joe.residence`尚未有值(為`nil`)時就將值指派給他，這時不會發生任何事，因為`joe.residence`仍為`nil`，必須直到他有一個`Residence`的實體後，才能正確指派值。

#### 呼叫方法

前面定義的類別中可以看到`printNumberOfRooms()`方法沒有返回值，前面章節提過，其實是會[返回一個 Void ](../ch1/functions.md#type)(即返回一個`()`的值或稱為空的元組`tuple`)。

這時在可選鏈中呼叫這個方法的話，則是會返回`Void?`，而不是`Void`(因為可選鏈一定會返回可選值)，所以仍然可以依照是否返回`nil`來判斷這個可選鏈是否成功，如下：

```swift
let kevin = Person()
kevin.residence = Residence()

// 可以依據是否返回 nil 來判斷可選鏈是否成功
if kevin.residence?.printNumberOfRooms() != nil {
    kevin.residence?.printNumberOfRooms()
} else {
    print("無法呼叫函式")
}

// 這時因為都有值且有預設值 所以會印出：有 0 間房間

```

#### 存取下標

與屬性相同，你可以使用可選鏈存取下標，也可以指派新的值給下標。底下是一個例子：

```swift
// 先生成一個實體
let kevin = Person()

// 這邊嘗試存取放在陣列中第一個房間的名稱
if let firstRoomName = kevin.residence?[0].name {
    print("第一個房間的名稱為 \(firstRoomName).")
} else {
    print("無法取得第一個房間的名稱")
}

// 這邊嘗試用下標指派一個值
kevin.residence?[0] = Room(name: "臥房")

// 上面兩個返回的都是 nil 因為目前 kevin.residence 尚未有值

// 先生成一個 Residence 實體
let kevinHouse = Residence()

// 接著為其內型別為 [Room] 的陣列屬性加上值
kevinHouse.rooms.append(Room(name: "廚房"))
kevinHouse.rooms.append(Room(name: "浴室"))

// 最後將其設置為 kevin.residence
kevin.residence = kevinHouse

// 這時再存取 即會有值 這邊會印出：第一個房間名稱為 廚房
if let firstRoomName = kevin.residence?[0].name {
    print("第一個房間名稱為 \(firstRoomName)")
} else {
    print("無法取得第一個房間的名稱")
}

```

##### Hint

- 如果可選鏈中的下標的值為一個可選值，記得要將問號`?`放在中括號`[]`的前面而不是後面。可選鏈的問號一定是緊接在可選表達式的後面。

#### 存取可選型別的下標

這邊以一個字典`Dictionary`型別的變數做示範，可選鏈的問號`?`必須放在下標的中括號`[]`後面來鏈接可選返回值，如下：

```swift
// 宣告一個字典的變數
var testScores = [
    "Dave": [86, 82, 84],
    "Bev": [79, 94, 81]
]

// 為下標設置新的值
testScores["Dave"]?[0] = 91
testScores["Bev"]?[0] += 1

// 因為沒有 Brian 這個鍵 會是 nil 
// 所以指派值會失敗 底下這行不會有任何事情發生
testScores["Brian"]?[0] = 72

```

#### 可選鏈的方法返回一個可選值

前面的例子說明可以使用可選鏈來存取可選型別的值，同樣地，也可以使用可選鏈來呼叫一個會**返回可選型別的值**的方法，如果需要的話仍然可以對該返回值繼續進行鏈接，如下：

```swift
john.someProperty?.someMethod()?.hasPrefix("The")

```

##### Hint

- 可選鏈的問號`?`是放在方法的小括號`()`後面，因為可選值是`someMethod()`方法的返回值，而不是`someMethod()`方法本身。

