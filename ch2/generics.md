# 泛型

泛型(`generic`)是 Swift 一個重要的特性，可以讓你自定義出一個**適用任意型別**的函式及型別。可以避免重複的程式碼且清楚的表達程式碼的目的。

許多 Swift 標準函式庫就是經由泛型程式碼建構出來的，像是陣列(`array`)和字典(`dictionary`)都是泛型的，你可以宣告一個`[Int]`陣列，也可以宣告一個`[String]`陣列。同樣地，你也可以宣告任意指定型別的字典。

你可以將泛型使用在函式、列舉、結構及類別上。

以下是一個例子：

```swift
// 定義一個將兩個整數變數的值互換的函式
func swapTwoInts(inout a: Int, inout _ b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}

// 定義兩個整數變數 並當做參數傳入函式
var oneInt = 12
var anotherInt = 500
swapTwoInts(&oneInt, &anotherInt)

print("互換後的 oneInt 為 \(oneInt)，anotherInt 為 \(anotherInt)")
// 印出：互換後的 oneInt 為 500，anotherInt 為 12

// 與上面定義的函式功能相同 只是這時互換的變數型別為字串
func swapTwoStrings(inout a: String, inout _ b: String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

```

由上述程式可以看到，兩個函式的功能完全一樣，唯一不同的只有傳入參數的型別，

