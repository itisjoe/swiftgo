# 屬性

屬性(`property`)為特定型別(類別、結構或列舉)的值，有以下幾種使用方式：

- 儲存屬性(`stored property`)：在實體內儲存常數或變數，可以用於類別及結構。
- 計算屬性(`computed property`)：在實體內計算一個值，可以用於類別、結構及列舉。
- 型別屬性(`type property`)：與前兩個不同，這是屬於**型別本身**的屬性。
- 屬性觀察器(`property observer`)：用來觀察屬性值的變化，並以此觸發一個自定義的操作。


### 儲存屬性 Stored Properties

**儲存屬性**就是一個儲存在特定型別(類別或結構)的常數或變數。可以在定義儲存屬性時指定預設值，也可以在建構過程中設置或修改儲存屬性的值，以下是個例子：

```swift
// 定義一個遊戲角色的血量與法力最大值
struct CharacterStats {
    var hpValueMax: Double = 300 // 指定一個預設值
    let mpValueMax: Double
}

// 或是在建構時設置屬性的值
var oneStats = CharacterStats(hpValueMax: 500, mpValueMax: 120)

// 生成實體後也可以再修改屬性的值
oneStats.hpValueMax = 200

// 但因為 mpValueMax 為一個結構裡的常數屬性 所以不能修改常數 下面這行會報錯誤
oneStats.mpValueMax = 200

```

#### 常數結構的儲存屬性

如果生成一個結構的實體並指派給一個常數，則無法修改這個實體的任何屬性，就算是結構裡的儲存屬性為變數也無法，例子如下：

```swift
// 這邊使用前面定義的 CharacterStats 結構
// 生成一個 CharacterStats 結構的實體 並指派給一個常數 someStats
let someStats = CharacterStats(hpValue: 900, mpValue: 150)

// 這個實體 someStats 為一個常數 所以即使 hpValue 為一個變數屬性 仍然不能修改這個值 這行會報錯誤
someStats.hpValue = 1200

```

前面章節有提到過，結構(`struct`)是屬於值型別(`value type`)，所以當實體宣告為常數時，其內所有屬性也就都是常數而無法修改了。

而相對地，類別(`class`)是屬於參考型別(`reference type`)，一個類別實體的常數，仍可以修改其內的屬性，因為這時候這個常數儲存的是參考(參考其在記憶體空間內配置的位置)，而不是儲存這個實體。


#### 延遲儲存屬性 Lazy Stored Properties

延遲儲存屬性是指當第一次被呼叫的時候才會計算其初始值的屬性。在屬性宣告前使用`lazy`來表示一個延遲儲存屬性。

##### Hint

- 延遲儲存屬性只能使用在變數，因為屬性的值在實體建構完成之前可能無法得到，而常數屬性在建構完成之前必須要有初始值。

使用延遲儲存屬性可以讓類別中如果需要大量計算才能初始化的屬性，在需要的時候才真的初始化它，以下是個例子：

```swift
// 首先定義一個類別 DataImporter 
// 這個類別會導入外部檔案並執行一些操作 初始化可能會花費不少時間
class DataImporter {
    // 這邊簡化成一個檔案名稱 實際上可能會有很多操作
    var fileName = "data.txt"
}

// 接著定義另一個類別 DataManager
class DataManager {
    // 延遲儲存屬性
    lazy var importer = DataImporter()
    // 操作時需要用到的資料
    var data = [String]()

    // 簡化內部內容 可能還有許多操作資料的動作
}

// 生成一個類別 DataManager 的實體常數
let manager = DataManager()

// 添加一些資料
manager.data.append("Some data")
manager.data.append("Some more data")

// 到目前為止 manager 的 importer 都尚未被初始化

// 直到第一次使用這個屬性 才會被創建並初始化
print(manager.importer.fileName)

```


### 計算屬性 Computed Properties

除了儲存屬性外，類別、結構和列舉還可以定義**計算屬性**，計算屬性不直接儲存值，而是提供一個`getter`(使用關鍵字`get`)來存取值，及一個可選的`setter`(使用關鍵字`set`)來間接設置其他屬性的值。以下是個例子：

```swift
// 定義一個遊戲角色的狀態
class GameCharacter {
    // 血量初始值
    var hpValue: Double = 100
    
    // 防禦力初始值
    var defenceValue: Double = 300
    
    // 總防禦力的 getter 跟 setter
    var totalDefence: Double {
        get {
            // 總防禦力的算法是 防禦力加上 10% 血量
            return (defenceValue + 0.1 * hpValue)
        }
        set(levelUp) {
            // 升級時 會將血量及防禦力乘上一個倍數
            hpValue = hpValue * (1 + levelUp)
            defenceValue = defenceValue * (1 + levelUp)
        }
    }
    
}

// 生成一個類別 GameCharacter 的實體常數 oneChar
let oneChar = GameCharacter();

// 取得目前角色的總防禦力
print(oneChar.totalDefence) // 印出 310.0

// 升級時 角色狀態各數值會乘上的倍數 0.05
oneChar.totalDefence = 0.05

// 則現在角色的血量與防禦力會變成 105 跟 315
print("血量：\(oneChar.hpValue), 防禦力：\(oneChar.defenceValue)") // 印出 血量：105.0, 防禦力：315.0

```

#### 簡化 setter 

可以將`setter`簡化，省略掉傳入的參數時，Swift 會提供一個內建的參數名稱`newValue`。以下將前面定義的類別`GameCharacter`中的`setter`簡化：

```swift
// 將原先的 levelUp 參數移除 這時會提供一個內建的參數名稱 newValue
        set {
            // 升級時 會將血量及防禦力乘上一個倍數
            hpValue = hpValue * (1 + newValue)
            defenceValue = defenceValue * (1 + newValue)
        }

```

#### 唯讀計算屬性

計算屬性的`setter`是可選的，所以依照需求可以只寫`getter`，這時可以將計算屬性簡化，以下修改自前面定義的類別`GameCharacter`：

```swift
// 定義一個遊戲角色的狀態
class GameCharacter {
    // 血量初始值
    var hpValue: Double = 100
    
    // 防禦力初始值
    var defenceValue: Double = 300
    
    // 總防禦力只有 getter
    var totalDefence: Double {
        // 總防禦力的算法是 防禦力加上 10% 血量
        return (defenceValue + 0.1 * hpValue)
    }
    
}

```

上述程式中，因為計算屬性只有`getter`，所以`getter`可以省略掉關鍵字`get`及大括號`{}`。


### 屬性觀察器 Property Observers

屬性觀察器會監控和回應屬性值的變化，每次屬性被設置新的值都會呼叫屬性觀察器。以下為兩個可以使用的屬性觀察器：

- `willSet`：在設置新的值之前呼叫，會將這個新的值當做一個常數參數傳入，如果不命名這個參數名稱時，會有一個內建的參數名稱`newValue`。
- `didSet`：在新的值被設置之後立即呼叫，會將舊的屬性值當做參數傳入，這個參數可以自己命名，或直接使用內建的參數名稱`oldValue`。

以下是一個例子：

```swift
// 定義一個遊戲角色的狀態
class GameCharacter {
    // 血量初始值
    var hpValue: Double = 100 {
        willSet (hpChange) {
            // 改變血量前
            print("新的血量為\(hpChange)")
        }
        didSet {
            // 改變血量後
            if oldValue > hpValue {
                // 原血量較高 所以是受到攻擊 損血
                print("我損血了！哦阿！")
            } else {
                print("我補血了！耶！")
            }
        }
    }
    
}

// 生成一個類別 GameCharacter 的實體常數 oneChar
let oneChar = GameCharacter();

// 角色受到攻擊 血量降低
oneChar.hpValue = 90 // 因為有 willSet 所以會印出 新的血量為90.0

// 設置完新的血量後 因為有 didSet 所以會印出 我損血了！哦阿！

```

上述程式中的`willSet`有命名參數名稱`hpChange`，所以其內是使用`hpChange`，而`didSet`沒有命名參數名稱，所以其內是使用內建的參數名稱`oldValue`。

##### Hint

- 如果屬性經由輸入輸出參數(`In-Out Parameters`)方式傳入函式，，`willSet`和`didSet`也一樣會被觸發。


### 型別屬性 Type Properties

型別屬性是屬於這個**型別**(類別、結構或列舉)的屬性，無論生成了多少這個型別的實體，型別屬性都只有唯一一份。

型別屬性使用於定義所有從這個型別生成的實體共享的資料。

##### Hint

- 儲存型的型別屬性一定要有預設值，因為型別本身沒有建構器，無法在初始化過程中設值給型別屬性。
- 儲存型的型別屬性是延遲初始化的，只有在第一次被呼叫時才會被初始化，所以不需要對其使用`lazy`。

型別屬性是使用`static`關鍵字作宣告變數或常數。

在為類別宣告計算型的型別屬性時，依照需求可以改用關鍵字`class`來支持子類別對父類別的實作覆寫(`override`)，也就是在將一個**類別A**的計算型的型別屬性以`class`宣告後，之後新的**類別B**繼承這個**類別A**時，可以覆寫這個型別屬性。

設置儲存型跟計算型的型別屬性如下：

```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value in structure."
    static var computedTypeProperty: Int {
        return 1
    }
}
enum SomeEnumeration {
    static var storedTypeProperty = "Some value in enumeration."
    static var computedTypeProperty: Int {
        return 6
    }
}
class SomeClass {
    static var storedTypeProperty = "Some value in class."
    static var computedTypeProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}

```

上述程式中，計算型的型別屬性只有`getter`，實際上可以定義有`getter`跟`setter`的計算型的型別屬性，使用方法與先前介紹的計算型屬性相同。

後面章節會正式介紹繼承(子類別與父類別的關係)。

#### 存取或設置型別屬性的值

與實體的屬性一樣，型別屬性的存取也是使用點語法(`dot syntax`)，但是型別屬性是向**型別本身**存取和設置，而不是向**實體**，例子如下：

```swift
// 這邊使用前面定義的結構 SomeStructure, 列舉 SomeEnumeration, 類別 SomeClass

print(SomeStructure.storedTypeProperty) // 印出 Some value in structure.

// 設置一個型別屬性
SomeStructure.storedTypeProperty = "Another value."
print(SomeStructure.storedTypeProperty) // 印出 Another value.

print(SomeEnumeration.computedTypeProperty) // 印出 6
print(SomeClass.computedTypeProperty) // 印出 27

```

上述程式可以看到，要使用一個型別屬性時，是不需要生成任何的實體就可以使用的。

