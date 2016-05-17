# 圖片 UIImageView

顯示圖片主要是使用 UIImageView 及 UIImage 來完成，本節會介紹兩個範例，第一個範例會示範三種建立 UIImageView 的方式以及顯示模式，第二個範例則會示範一個自動輪播圖片的功能。


### 建立 UIImageView

這個範例的目標如下圖：

![uiimageview01](../images/uikit/uiimageview/uiimageview01.png)

首先在 Xcode 裡，[新建一個 **Single View Application** 類型的專案](../more/open_project.md#create_a_new_project)，取名為 ExUIImageView 。

一開始先在`viewDidLoad()`中取得螢幕尺寸，以供後續使用，如下：

```swift
// 取得螢幕的尺寸
let fullScreenSize = UIScreen.mainScreen().bounds.size

```

▼ 第一個建立 UIImageView 的方式為使用`UIImageView(frame:)`：

```swift
// 使用 UIImageView(frame:) 建立一個 UIImageView
var myImageView = UIImageView(
  frame: CGRect(
    x: 0, y: 0, width: 100, height: 100))

// 使用 UIImage(named:) 放置圖片檔案
myImageView.image = UIImage(named: "01.jpg")

// 設置新的位置並放入畫面中
myImageView.center = CGPoint(
  x: fullScreenSize.width * 0.5,
  y: fullScreenSize.height * 0.15)
self.view.addSubview(myImageView)

```

上述程式可以看到，`UIImageView`不是直接放置圖片檔案名稱，而是要藉由`UIImage(named:)`建立，再設置給`UIImageView`的屬性`image`，未來其他元件需要使用圖片時，也都是使用`UIImage(named:)`設置圖片檔案。

▼ 第二個建立 UIImageView 的方式為使用`UIImageView(image:)`，在初始化時直接給一個`UIImage`，後續再設定尺寸：

```swift
// 使用 UIImageView(image:) 建立一個 UIImageView
myImageView = UIImageView(
  image: UIImage(named: "02.jpg"))

// 設置原點及尺寸
myImageView.frame = CGRect(
  x: 0, y: 0, width: 100, height: 100)

// 設置新的位置並放入畫面中
myImageView.center = CGPoint(
  x: fullScreenSize.width * 0.5,
  y: fullScreenSize.height * 0.35)
self.view.addSubview(myImageView)

```

▼ 第三個建立 UIImageView 的方式為使用`UIImageView(image:, highlightedImage:)`，在初始化時除了直接設置一個`UIImage`外，還設置了一個 highlighted 狀態時的`UIImage`：

```swift
// 使用 UIImageView(image:, highlightedImage:)
// 建立一個 UIImageView
myImageView = UIImageView(
  image: UIImage(named: "02.jpg"),
  highlightedImage: UIImage(named: "03.jpg"))

// 設置原點及尺寸
myImageView.frame = CGRect(
  x: 0, y: 0, width: 100, height: 100)

// 設置圖片 highlighted 狀態
myImageView.highlighted = true

// 設置新的位置並放入畫面中
myImageView.center = CGPoint(
  x: fullScreenSize.width * 0.5,
  y: fullScreenSize.height * 0.55)
self.view.addSubview(myImageView)

```

上面三個 UIImageView 的圖片剛好都為正方形，所以可以剛好適合視圖的尺寸，而如果當圖片尺寸與 UIImageView 的尺寸不一樣時，就需要使用 UIImageView 的一個屬性`contentMode`來設置顯示模式：

```swift
// 建立一個 UIImageView
myImageView = UIImageView(
  image: UIImage(named: "04.jpg"))
myImageView.frame = CGRect(
  x: 0, y: 0, width: 200, height: 200)

// 設置背景顏色
myImageView.backgroundColor = UIColor.yellowColor()

// 設置圖片顯示模式
myImageView.contentMode = .BottomLeft

// 設置新的位置並放入畫面中
myImageView.center = CGPoint(
  x: fullScreenSize.width * 0.5,
  y: fullScreenSize.height * 0.8)
self.view.addSubview(myImageView)

```

上述程式可以看到 UIImageView 的尺寸為`200x200`，而圖片的尺寸為`240x159`，所以沒有辦法剛好放進去，這時就使用`contentMode`這個屬性來設定顯示模式，這邊示範設置為`.BottomLeft`，也就是以左下角為準。將 UIImageView 底色設為黃色，所以你可以看到最後會超出原本 UIImageView 設置的尺寸，並在上面露出了一些底色。

##### Hint

- 預設`contentMode`屬性的值為`ScaleToFill`，會自動縮放圖片以填滿 UIImageView 的尺寸。
- `contentMode`屬性其他可以設定的模式，還有**以長或寬為準的縮放**、**重新裁切**或是**以八個方向或是中心為準**等等，你可以自己測試看看。


### 圖片來源

- https://www.flickr.com/photos/134525588@N04/20344150965
- https://www.flickr.com/photos/voilaquoi/18911800758
- https://www.flickr.com/photos/nasacommons/5136519916/
- https://www.flickr.com/photos/136245400@N03/24183123165/


### 範例

本節範例程式碼放在 [uikit/uiimageview](https://github.com/itisjoe/swiftgo_files/tree/master/uikit/uiimageview)

