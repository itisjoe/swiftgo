# 巢狀型別

巢狀型別(`nested types`)表示在一個列舉、結構或類別中，還可以依照需求在其內，再定義列舉、結構或類別，以下是定義一個撲克牌結構，並內含列舉的例子：

```swift
struct Poker {

    enum Suit: String {
        case Spades = "黑桃", Hearts = "紅心", Diamonds = "方塊", Clubs = "梅花"
    }

    enum Rank: Int {
        case Two = 2, Three, Four, Five, Six, Seven, Eight, Nine, Ten
        case Jack, Queen, King, Ace
    }

    let rank: Rank, suit: Suit
    
    func description () {
        print("這張牌的花色是：\(suit.rawValue)，點數為：\(rank.rawValue)")
    }

}

let poker = Poker(rank: .King, suit: .Hearts)

// 印出：這張牌的花色是：紅心，點數為：13
poker.description()

```

如果你要在外部使用一個巢狀型別內部的列舉、結構或類別時，可以在其前面直接加上這個巢狀型別的名稱即可，如下：

```swift
let diamondsName = Poker.Suit.Diamonds

// 印出：方塊
print(diamondsName.rawValue)

```


### 範例

本節範例程式碼放在 [ch2/nested-types.playground](https://github.com/itisjoe/swiftgo_files/tree/master/ch2/nested-types.playground)

