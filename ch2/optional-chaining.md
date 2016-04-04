# 可選鏈

可選鏈(`optional chaining`)是一個可以存取或呼叫屬性(`property`)、方法(`method`)及下標(`subscript`)的過程。

稱呼其為**可選**(`optional`)是因為當前存取或呼叫的目標可能為空(`nil`)，而多次存取或呼叫可以用**點語法**(`dot syntax`，即`.`)將其全部連結在一起，所以稱為鏈(`chaining`)。

可選鏈中，只要其中一個節點為空(`nil`)，則會立即返回`nil`。相對地，如果存取或呼叫至最後一個節點都有值，則會返回一個可選型別的值。


### 以可選鏈替代強制解析

可選鏈中，如果其中一個節點為可選值時，必須在這個值後面加上問號(`?`)，來表示這是一個可選值，也就是當這個值為空(`nil`)時，會立即返回`nil`。

與強制解析(`forced unwrapping`)的在值後面加上驚嘆號(`!`)不同，強制解析的值如果為空(`nil`)時，會觸發程式運行錯誤。而可選鏈中的可選值(加上問號`?`)如果為空(`nil`)，則只會單純的返回`nil`。

不論可選鏈最後返回的屬性、方法或下標是不是可選型別的值，都是返回一個可選值，所以可以用來判斷這個可選鏈是否成功，有返回值是成功，返回`nil`則是失敗。像是原本返回應該是一個`Int`型別的值，經由可選鏈後返回會是一個`Int?`可選型別的值。

以下是一個例子：

```swift
// 定義一個類別 Person
class Person {
    
    // 有一個屬性為可選的 Residence 型別 residence
    var residence: Residence?
}

// 定義一個類別 Residence
class Residence {
    
    // 有一個屬性為 Int 型別 numberOfRooms
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

// 上述 if 語句目前會印出 無法取得房間數量
// 接著為 joe.residence 設置一個 Residence 實體
joe.residence = Residence()

// 這時就會返回 1
// 記得這是返回一個可選型別 Int?
print(joe.residence?.numberOfRooms)

```


### 通過可選鏈存取屬性

前面的例子就是示範存取屬性，還要提的一點是，你可以嘗試通過可選鏈來給屬性指派值，如下：

```swift
// 使用上面生成的實體 joe 設置新的值
joe.residence?.numberOfRooms = 12

```

何以說**嘗試**指派值，因為如果`joe.residence`尚未有值(為`nil`)時就將值指派給他，這時不會發生任何事，因為`joe.residence`仍為`nil`，需要直到他有一個`Residence`的實體後，才能正確指派值。

### 通過可選鏈呼叫方法及存取下標

接著將前面例子中的`Residence`重新定義，並新增一個類別`Room`，類別`Person`則是沿用，如下：

```swift
// 定義一個類別 Room
class Room {

    // 有一個屬性為 String 型別 name
    let name: String

    // 生成實體時同時設置屬性 name 的值
    init(name: String) { self.name = name }
}

// 重新定義類別 Residence
class Residence {

    // 新增一個屬性 型別為 [Room] 的陣列 預設值是空陣列
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

首先示範呼叫方法，可以看到`printNumberOfRooms()`方法沒有返回值，前面章節提過，其實是會返回一個`Void`(即返回一個`()`的值或稱為空的元組`tuple`)。

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

// 這時因為都有值且有預設值 所以會印出： 有 0 間房間

```







