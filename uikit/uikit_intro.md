# UIKit 介紹

從這節起，我們會開始介紹如何使用 UIKit 框架來建構一個應用程式。UIKit 框架代表著把一些建構 UI 功能的工具集合在一起，可以讓你快速、方便的使用這些工具。

首先在 Xcode 裡，[新建一個 **Single View Application** 類型的專案](../more/open_project.md#create_a_new_project)，取名為 MyFirstProject 。

▼ 已經有預先建立一些檔案在專案裡面，如下圖：

![interface_intro01](../images/interface_intro/interface_intro01.png)

首先看到有兩隻 .swift 檔案：`AppDelegate.swift`及`ViewController.swift`，程式主要都是寫在這兩隻檔案裡面。

應用程式開啟時，會自`AppDelegate.swift`開始，這隻檔案負責應用程式的生命週期，像是啟動、閒置、進入後台、返回前台或是退出時要執行的動作。

接著看到 ViewController.swift ，是應用程式預設的主要視圖(`View`)控制器(`Controller`)，所有需要的 UI 功能(像是按鈕、文字或圖案等等)，都必須在這個 ViewController 裡面建立。

要如何建立 UI (`User Interface`使用者介面)呢？就是要用到內建的許多 UIKit 元件。

### 隨處可見的 UIKit

在使用 iPhone 的經驗裡，你可能會發現大多數應用程式的樣貌及使用方式都差不多，除了 Apple 官方提供了一些 UI 規則給開發者遵循之外，其實是因為大多數功能都是使用內建的 UIKit 元件就可以完成的。

▼ iPhone 的**設定 App **就是由許多的 UIKit 元件所組成，像是`UINavigationController`、`UITableView`及`UIImageView`等等，如下圖：

![uikit_intro](../images/uikit_intro/uikit_intro01.png)

你也許可以觀察得到，這些元件都以`UI`為開頭，這是在 UIKit 設計時便特地命名的(也可以說是一種習慣)，讓你可以很輕易、清楚的明白你現在使用的東西的主要目的：這些元件都是用來建構 UI 的。

##### Hint

- 在往後的學習中，也可能會遇到一些有相同縮寫字母開頭的函式或類別，這都是在建立這些功能時特地命名的，所以你可以很清楚的知道哪些函式是用於同一種類型的功能。


### 建立第一個 UIKit 元件

最開頭的一開始，我們先介紹最基礎的一個元件：`UIView`，所有 UIKit 的元件都是繼承自 UIView ，所以 UIView 其實很單純，只是有著每個元件都需要的、最基礎的屬性及方法。

在說明如何將`UIView`放進畫面之前，要先介紹另一個工具：`UIScreen`，這主要是用來代表螢幕的資訊，通常是用來取得整個螢幕的尺寸，如下：

```swift
let fullScreenSize = UIScreen.mainScreen().bounds.size

```

`UIScreen.mainScreen()`表示的是主畫面的資訊，其內有一個屬性為`bounds`，`bounds`又包含了兩個主要屬性：`origin`及`size`，分別是主畫面的**起始點**及**尺寸**。

而往後都會利用這個主畫面的尺寸來為每個元件設置相對的位置。

##### Hint

- 因為本書所有畫面都是用純程式碼構成，為了可以較為彈性的適用各尺寸 iPhone ，所以大多會以相對於整個螢幕畫面尺寸來設置 UIKit 元件的大小及位置。






