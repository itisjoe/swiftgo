# 滑動視圖 UIScrollView

當一個元件的**實際視圖範圍**比**可見視圖範圍**大時，會加上滑動條( scroll bar )讓使用者自由滑動，UIScrollView 就是有這一特性的最基本元件。實際上，前面章節介紹過的 [UITextView](../uikit/uitextview.md) 、 [UITableView](../uikit/uitableview.md) 及 [UICollectionView](../uikit/uicollectionview.md) 都是繼承自 UIScrollView ，所以他們也都繼承了 UIScrollView 的特性。

本節會先介紹如何建立一個基本的 UIScrollView 並介紹常用的屬性及方法，接著會配合 UIPageControl 元件建立像是新手導覽向右滑動的分頁功能。


