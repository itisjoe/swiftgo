# 存取控制

- [存取層級](#access_levels)
- [自定義型別](#custom_type)
- [子類別](#subclass)
- [常數、變數、屬性及下標](#variable)
- [建構器](#initializer)
- [協定](#protocol)
- [擴展](#extension)
- [泛型](#generic)
- [型別別名](#typealias)

Swift 提供存取控制(`access control`)的特性，讓你可以為程式碼或模組設置存取權限，決定哪些部分可以開放給外部程式碼使用。

Swift 中，可以為列舉、結構或類別設置存取權限，這些型別內部的屬性、方法、下標或建構器也可以設置存取權限。而協定中的全域變數、常數或函式也可以設置存取權限。

如果你只是單純的在開發一個獨立的應用程式，而不是開發一個模組(像是可供他人使用的工具)，其實可以不用顯式地設置存取權限，Swift 已為大部分的情況提供預設存取權限。

<a name="access_levels"></a>
### 存取層級

Swift 提供了 3 種不同的存取層級，分別如下：

- public：公開存取的層級，同模組中的任何程式及外部引用這個模組的程式都可以使用。通常要將一個模組的介面(與模組以外程式互動交流的部分)設為公開時，會將其設為`public`。
- internal：內部存取的層級，同模組中的程式可以使用，但外部引用的程式不能使用。當定義為只供應用程式或模組內部使用時，可以將其設為`internal`。
- private：私有存取的層級，只能在原始定義的程式碼中使用。使用`private`可以用來隱藏特定功能的實作。

使用方式為在變數、函式或類別的前面加上這 3 種不同的關鍵字來宣告它們的存取層級，如下：

```swift
public class SomePublicClass {}
internal class SomeInternalClass {}
private class SomePrivateClass {}

public var somePublicVariable = 0
internal let someInternalConstant = 0
private func somePrivateFunction() {}

```

#### 存取層級基本原則

Swift 存取層級的基本原則：不可以在一個實體中定義存取層級限制更嚴格的實體。如以下的例子：

- 一個`public`的變數，不能將它的型別定義為`internal`或`private`。因為當變數可以被公開存取，但定義它的型別不行，這樣會出現錯誤。
- 一個函式的參數、返回型別存取層級限制不能比函式本身的更嚴格。因為當函式存取層級設為`public`，可以被公開存取時，但參數或返回型別存取層級設為`internal`或`private`，無法被公開存取，這樣會出現錯誤。

#### 預設存取層級

如果沒有顯性宣告存取層級時，`internal`就是預設的存取層級。

<a name="custom_type"></a>
### 自定義型別

你也可以為自定義的型別定義存取層級，當然你必須確認這個型別的作用範圍與存取層級的限制相符。

類別的存取層級會影響其內部成員的存取層級，像是定義一個`private`的類別，則其內部成員的存取層級都會是`private`。而定義一個`public`或`internal`的類別，其內部成員都會是`internal`。

##### Hint

- 一個`public`類別，其內成員預設存取層級為`internal`而不是`public`，如果要為某個成員設為`public`，必須顯示定義。這樣在你定義一個模組的公開介面時，可以明確的選擇哪些介面是公開的，避免不小心將內部使用的介面公開。

以下是幾個類別與其內部成員存取層級的例子：

```swift
// 顯式指定為 public 類別
public class SomePublicClass {
    // 顯示指定為 public 成員
    public var somePublicProperty = 0

    // 隱式推斷為 internal 成員
    var someInternalProperty = 0
    
    // 顯示指定為 private 成員
    private func somePrivateMethod() {}
}

// 隱式推斷為 internal 類別
class SomeInternalClass {
    // 隱式推斷為 internal 成員
    var someInternalProperty = 0
    
    // 顯示指定為 private 成員
    private func somePrivateMethod() {}
}

// 顯式指定為 private 類別
private class SomePrivateClass {
    // 隱式推斷為 private 成員
    var somePrivateProperty = 0

    // 隱式推斷為 private 成員
    func somePrivateMethod() {}
}

```

#### 元組型別

元組不能明確指定存取層級，而是在使用時被自動推斷的。元組的存取層級會根據其內成員存取層級最嚴格的一個為準，例子如下：

```swift
private var description = "Sunny day !"
internal var number = 300
public var name = "Joe Black"

// 這時這個元組的存取層級會根據最嚴格的 private 為準
let someTuple = (description, number, name)

```

#### 函式型別

函式的存取層級是根據存取層級最嚴格的**參數或返回值型別**來決定。經由此規則得到的存取層級如果與預設存取層級不一樣，則必須明確的指定函式的存取層級，如下：

```swift
// 定義一個用來當做[函式返回值型別]的類別
private class SomeClass {}

// 定義一個函式
// 返回值為 SomeClass 存取層級為 private
// 則這個函式的存取層級也為 private
// 這時與預設的 internal 不一樣 所以必須明確指定函式為 private
// 如果將函式前面的 private 拿掉 會報錯誤
private func someFunction() -> SomeClass {
    return SomeClass()
}

```

#### 列舉型別

列舉成員的存取層級與列舉相同，無法為列舉成員單獨指定不同的存取層級，如下：

```swift
// 定義列舉的存取層級為 public
public enum CompassPoint {
    // 則列舉成員都為 public
    case North
    case South
    case East
    case West
}

```

而列舉的原始值(`raw value`)與相關值(`associated value`)的存取層級限制不能比列舉的存取層級嚴格。例如，你不能在一個`internal`的列舉中定義`private`的原始值。

#### 巢狀型別

以巢狀型別來說，定義在`private`型別中的巢狀型別，會自動指定為`private`。而在`public`或`internal`型別中，巢狀型別則自動指定為`internal`，這時如果要指定巢狀型別為`public`，則必須明確指定為`public`。

<a name="subclass"></a>
### 子類別

子類別的存取層級限制不能比父類別更為寬鬆，例如父類別為`internal`時，子類別就不能是`public`。

在符合當前存取層級限制的條件下：

- 子類別可以覆寫父類別任意的成員(方法、屬性、建構器或下標等)，用來提供限制較寬鬆的存取層級。
- 子類別成員可以存取限制更嚴格的父類別成員(因為子類別與父類別定義為同一個原始程式碼中)。

以下是一個例子：

```swift
// 定義一個 public 的類別 A
public class A {
    // 定義一個 private 的方法
    private func someMethod() {}
}

// 繼承自 A 的類別 B 其存取層級為 internal
// 符合 子類別的存取層級限制不能比父類別更為寬鬆
internal class B: A {
    // 可以覆寫父類別的方法 更新為較寬鬆的存取層級
    // (當然必須符合自身的存取層級)
    override internal func someMethod() {
        // 可以呼叫 存取層級限制更嚴格的父類別成員
        super.someMethod()
    }
}

```

<a name="variable"></a>
### 常數、變數、屬性及下標

常數、變數及屬性不能擁有比它們的**型別**限制更為寬鬆的存取層級。例如，不能定義一個`public`的屬性，但它的型別卻是`private`。而下標也不能擁有比它們的**索引值或返回值型別**限制更為寬鬆的存取層級。以下是一個例子：

```swift
// 定義一個 private 的類別
private class SomeClass {}

// 這時變數的型別為 private 則變數必須顯式的定義為 private
// 將 private 拿掉會報錯誤 因為會變成預設的 internal 則與規則不符
private var someInstance = SomeClass()

```

#### Getter 與 Setter

常數、變數、下標與屬性的`Getter`和`Setter`的存取層級與他們所屬型別的存取層級相同。

`Setter`的存取層級可以比對應的`Getter`存取層級限制更為嚴格，可以用來控制其讀寫權限。使用方式為在`var`或`subscript`關鍵字前，加上`private(set)`或`internal(set)`來指定限制更為嚴格的存取層級。以下是一個例子：

```swift
// 定義一個結構 預設存取層級為 internal
struct TrackedString {
    // 將變數的 Setter 存取層級指定為 private
    private(set) var numberOfEdits = 0
    var value: String = "" {
        didSet {
            // 所以在結構內部 是可以讀寫的
            numberOfEdits += 1
        }
    }
}

// 宣告一個結構的變數
var stringToEdit = TrackedString()

// 每修改一次 會經由屬性觀察器來將內部變數屬性加一
stringToEdit.value = "字串修改次數會被記錄"
stringToEdit.value += "每修改一次, numberOfEdits 數字會加一"
stringToEdit.value += "這行修改也會加一"

// 印出：已修改了 3 次
print("已修改了 \(stringToEdit.numberOfEdits) 次")

```

你也可以在必要時為`Getter`與`Setter`顯式指定存取層級，例子如下：

```swift
// 顯式指定這個結構的存取層級為 public
public struct TrackedString {
    // 結合 public 與 private(set)
    // 所以這時這個變數屬性的 Setter 為 private
    // Getter 為 public
    public private(set) var numberOfEdits = 0
    public var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
    public init() {}
}

```

<a name="initializer"></a>
### 建構器

自定義建構器的存取層級限制不能比其所屬型別的存取層級寬鬆，像是一個`internal`的類別，不能設置一個`public`的建構器。而唯一例外是，當建構器為**必要建構器**(`required initializer`)時，其存取層級必須與所屬型別相同。

與函式或方法一樣，建構器參數的存取層級限制也不能比建構器本身嚴格。像是一個`internal`的建構器，不能設置一個`private`的參數。

#### 預設建構器

預設建構器的存取層級與所屬型別的存取層級相同。除非當型別的存取層級為`public`，則預設建構器會被設置為`internal`，如果需要一個`public`的建構器，必須自己定義一個，如下：

```swift
public class SomeClass {
    public init() {}
}

```

#### 結構的成員逐一建構器

如果結構中任意儲存屬性的存取層級為`private`，那麼這個結構預設的成員逐一建構器的存取層級就是`private`，否則就為`internal`。

如果需要在其他模組也可以使用這個結構的成員逐一建構器，則必須自行定義一個`public`的成員逐一建構器。

<a name="protocol"></a>
### 協定

如果想為一個協定明確的指定存取層級，必須在定義此協定時指定。這樣可以確保這個協定只能在適當的存取層級範圍內被遵循。

協定中的每個功能都與該協定的存取層級相同，這樣才能確保協定所有功能都可以被遵循此協定的型別存取。

#### 協定繼承

從已存在的協定繼承了一個新的協定時，這個新協定的存取層級不能比已存在協定的寬鬆。例如，定義一個`public`的協定時，不能繼承自一個`internal`的協定。

#### 協定一致性

一個型別可以遵循一個存取層級限制更為嚴格的協定，例如，你可以定義一個`public`的型別，並遵循一個`internal`的協定。

遵循了協定的型別的存取層級，會以**型別本身**與**遵循的協定**限制較嚴格的存取層級為準，如果一個`public`的型別，遵循了一個`internal`的協定，則在此型別作為符合協定的型別時，其存取層級也是`internal`。

當你讓一個型別遵循某個協定並滿足其所有要求後，你必須確保所有這些實作協定的部分，其存取層級不能比協定更為嚴格。例如一個`public`的型別，遵循了`internal`的協定，則實作協定的部份最嚴格只能到`internal`存取層級。

<a name="extension"></a>
### 擴展

你可以在符合存取層級的情況下，擴展一個列舉、結構或類別，這個擴展會與擴展的對象擁有一樣的存取層級。例如，你擴展了一個`public`或`internal`型別，擴展中的成員則預設為`internal`，與原始型別中的成員一樣。而當擴展了一個`private`型別時，擴展成員則預設為`private`。

或者，你也可以明確的指定擴展的存取層級，來讓其內成員都預設成一樣的存取層級。這個預設的存取層級仍可被個別成員所指定的存取層級覆蓋。

#### 經由擴展來遵循協定

如果你經由擴展來遵循協定，那你就不能顯式的指定這個擴展的存取層級了。而這個協定本身的存取層級會變成預設的存取層級，且擴展中每個協定功能的實作也是一樣的預設的存取層級。

<a name="generic"></a>
### 泛型

泛型型別或泛型函式的存取層級由**泛型型別或泛型函式本身**與**泛型的型別約束參數**中限制最嚴格的來確定。

<a name="typealias"></a>
### 型別別名

自定義的任何型別別名都會被當做不同的型別來做存取控制。型別別名不能擁有比原始型別限制更為寬鬆的存取層級。

- 一個`public`的型別，可以宣告`private`、`internal`或`public`的型別別名。
- 一個`private`的型別，僅能宣告`private`的型別別名，不能宣告為`internal`或`public`的型別別名。

