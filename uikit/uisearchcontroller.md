# 搜尋 UISearchController

當有一大筆資訊時，通常可以加上搜尋功能，用來篩選出需要的資訊，這節使用 UITableView 配合 UISearchController 來示範一個搜尋框功能，以下是本節目標：

![uisearchcontroller01](../images/uikit/uisearchcontroller/uisearchcontroller01.png)

首先在 Xcode 裡，[新建一個 **Single View Application** 類型的專案](../more/open_project.md#create_a_new_project)，取名為 ExUISearchController 。

一開始先為`ViewController`建立四個屬性：

```swift
class ViewController: UIViewController {
    var tableView: UITableView!
    var searchController: UISearchController!
    
    let cities = [
        "臺北市","新北市","桃園市","臺中市","臺南市",
        "高雄市","基隆市","新竹市","嘉義市","新竹縣",
        "苗栗縣","彰化縣","南投縣","雲林縣","嘉義縣",
        "屏東縣","宜蘭縣","花蓮縣","臺東縣","澎湖縣",]

    var searchArr: [String] = [String](){
        didSet {
            // 重設 searchArr 後重整 tableView
            self.tableView.reloadData()
        }
    }

    // 省略
}
```

以及在`viewDidLoad()`中取得螢幕尺寸，以供後續使用，如下：

```swift
// 取得螢幕的尺寸
let fullScreenSize = UIScreen.mainScreen().bounds.size

```

上述設置中，`cities`是全部原始的資料，而`searchArr`則是隨搜尋文字不同，篩選出來的資料，`didSet`的使用方法請參考[屬性觀察器](../ch2/properties.md#property_observer)。 UITableView 的方法`reloadData()`則是當資料有更新時重整表格用的。


### 建立 UISearchController

範例使用 UITableView 來配合搜尋結果，所以要先在`viewDidLoad()`中建立一個 UITableView ：

```swift
// 建立 UITableView
self.tableView = UITableView(frame: CGRect(
  x: 0, y: 20, 
  width: fullScreenSize.width,
  height: fullScreenSize.height - 20),
  style: .Plain)
self.tableView.registerClass(UITableViewCell.self,
  forCellReuseIdentifier: "Cell")
self.tableView.delegate = self
self.tableView.dataSource = self
self.view.addSubview(self.tableView)

```

詳細使用方式請參考[表格 UITableView](../uikit/uitableview.md)。

接著在`viewDidLoad()`中建立 UISearchController ：

```swift
// 建立 UISearchController 並設置搜尋控制器為 nil
self.searchController =
  UISearchController(searchResultsController: nil)

// 將更新搜尋結果的對象設為 self
self.searchController.searchResultsUpdater = self

// 搜尋時是否隱藏 NavigationBar
// 這個範例沒有使用 NavigationBar 所以設置什麼沒有影響
self.searchController
  .hidesNavigationBarDuringPresentation = false

// 搜尋時是否使用燈箱效果 (會將畫面變暗以集中搜尋焦點)
self.searchController
  .dimsBackgroundDuringPresentation = false

// 搜尋框的樣式
self.searchController.searchBar.searchBarStyle =
  .Prominent

// 設置搜尋框的尺寸為自適應
// 因為會擺在 tableView 的 header
// 所以尺寸會與 tableView 的 header 一樣
self.searchController.searchBar.sizeToFit()

// 將搜尋框擺在 tableView 的 header
self.tableView.tableHeaderView =
  self.searchController.searchBar

```


### 委任模式

除了 UITableView 有設置委任對象， UISearchController 也同樣設置了更新搜尋結果的對象，在前面的範例中已經示範過了[在`self`(也就是 ViewController 本身)](../uikit/uitextfield.md)以及[另外建立一個獨立檔案](../uikit/uipickerview.md)來實作委任對象的方法。這邊則介紹第三種方式，以**擴展**來實作委任的方法。

實際上就與在`self`裡實作一樣，但基於** Swift 擴展 ( extension )**的特性，可以對 ViewController 新增一個擴展，來將委任方式寫在另外一個擴展檔案中。

擴展的詳細說明請參考[前面章節介紹的內容](../ch2/extensions.md)。

請先以[新增檔案](../more/addfile.md)的方式新增一個檔案，命名為 ViewControllerExtensions ，不同的地方在於，請選擇`iOS > Source > Swift File`，如下圖：

![uisearchcontroller02](../images/uikit/uisearchcontroller/uisearchcontroller02.png)

建立好檔案後，開啟 ViewControllerExtensions.swift ，先將原本的`import Foundation`改為`import UIKit`，並在其中以擴展 ViewController 的方式，加上需要實作的委任方法：

```swift
extension ViewController: UITableViewDataSource {
    func tableView(tableView: UITableView,
      numberOfRowsInSection section: Int) -> Int {
        if (self.searchController.active) {
            return self.searchArr.count
        } else {
            return self.cities.count
        }
    }
    
    func tableView(tableView: UITableView,
      cellForRowAtIndexPath indexPath: NSIndexPath)
      -> UITableViewCell {
        let cell =
          tableView.dequeueReusableCellWithIdentifier(
          "Cell", forIndexPath: indexPath)
        
        if (self.searchController.active) {
            cell.textLabel?.text =
              self.searchArr[indexPath.row]
            return cell
        } else {
            cell.textLabel?.text = 
              self.cities[indexPath.row]
            return cell
        }
    }
}

extension ViewController: UITableViewDelegate {
    func tableView(tableView: UITableView,
      didSelectRowAtIndexPath indexPath: NSIndexPath){
        tableView.deselectRowAtIndexPath(
          indexPath, animated: true)
        if (self.searchController.active) {
            print(
            "你選擇的是 \(self.searchArr[indexPath.row])")
        } else {
            print(
            "你選擇的是 \(self.cities[indexPath.row])")
        }
    }
}

extension ViewController: UISearchResultsUpdating {
    func updateSearchResultsForSearchController(searchController: UISearchController){
        // 取得搜尋文字
        guard let searchText =
          searchController.searchBar.text else {
            return
        }
    
        // 使用陣列的 filter() 方法篩選資料
        self.searchArr = self.cities.filter(
          { (city) -> Bool in
            // 將文字轉成 NSString 型別
            let cityText:NSString = city
            
            // 比對這筆資訊有沒有包含要搜尋的文字
            return (cityText.rangeOfString(
              searchText, options:
      NSStringCompareOptions.CaseInsensitiveSearch).location)
            != NSNotFound
        })
    }
}
```

上述程式中，前兩個擴展用來實作 UITableView 委任的方法，需要注意的是 UISearchController 有一個屬性`active`，用來表示目前是否為搜尋狀態，當搜尋狀態時就是顯示搜尋後的結果`searchArr`，而非搜尋狀態時則是顯示所有資訊`cities`。

最後一個擴展則是用來實作更新搜尋結果的方法，首先是取得搜尋的文字，接著則是對原始資料的陣列使用`filter()`方法篩選資訊，以比對文字的方式來搜尋。

`guard`的使用方式請參考[提前退出](../ch1/control_flow.md#guard)。

以上即為本節範例的內容。


### 範例

本節範例程式碼放在 [uikit/uisearchcontroller](https://github.com/itisjoe/swiftgo_files/tree/master/uikit/uisearchcontroller)

