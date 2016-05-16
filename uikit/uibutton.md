# 按鈕 UIButton

按鈕是最常見的控制元件，按下按鈕後可以執行許多動作，例如送出表單、切換頁面等等，應用程式的互動幾乎都與 UIButton 有關，是不可缺少的元件。本節的目標如下：

![uibutton01](../images/uikit/uibutton/uibutton01.png)

首先在 Xcode 裡，[新建一個 **Single View Application** 類型的專案](../more/open_project.md#create_a_new_project)，取名為 ExUIButton 。

一開始先在`viewDidLoad()`中取得螢幕尺寸，以及設置`self.view`的底色，以供後續使用，如下：

```swift
// 取得螢幕的尺寸
let fullScreenSize = UIScreen.mainScreen().bounds.size

// 為基底的 self.view 設置底色
self.view.backgroundColor = UIColor.whiteColor()

```

### 建立預設樣式的按鈕

UIKit 提供幾個內建預設樣式的按鈕，都是系統中可以看得到的樣式，如果你有一樣的需求，可以直接使用這樣的方式建立按鈕，如下：

```swift
// 預設的按鈕樣式
var myButton = UIButton(type: .ContactAdd)
myButton.center = CGPoint(
  x: fullScreenSize.width * 0.4,
  y: fullScreenSize.height * 0.2)
self.view.addSubview(myButton)

myButton = UIButton(type: .InfoLight)
myButton.center = CGPoint(
  x: fullScreenSize.width * 0.6,
  y: fullScreenSize.height * 0.2)
self.view.addSubview(myButton)

```

上述程式分別建立了加號`+`及驚嘆號`!`的按鈕(請參考上圖)，這邊可以注意到，這兩個按鈕用的是同一個變數`myButton`，因為各別都使用了`UIButton(type: .InfoLight)`來初始化一個按鈕，所以可以這樣使用。當然你也可以分別建立兩個常數(像是`let myButton1, myButton2`這樣)。


### 建立自定義的按鈕

如果要建立自定義的按鈕，則是使用`UIButton(frame:)`，如下：

```swift
// 使用 UIButton(frame:) 建立一個 UIButton
myButton = UIButton(
  frame: CGRect(x: 0, y: 0, width: 100, height: 30))

// 按鈕文字
myButton.setTitle("按我", forState: .Normal)

// 按鈕文字顏色
myButton.setTitleColor(UIColor.whiteColor(), forState: .Normal)

// 按鈕是否可以使用
myButton.enabled = true

// 按鈕背景顏色
myButton.backgroundColor = UIColor.darkGrayColor()

// 按鈕按下後的動作
myButton.addTarget(
	self, 
	action: #selector(ViewController.clickButton),
	forControlEvents: .TouchUpInside)

// 設置位置並加入畫面
myButton.center = CGPoint(
	x: fullScreenSize.width * 0.5,
    y: fullScreenSize.height * 0.5)
self.view.addSubview(myButton)

```

UIControl

addTarget(target:, action:, forControlEvents:)

    target參數為指定當事件發生時呼叫那一個物件
    action參數為該物件的方法
    controlEvent:觸發的事件

