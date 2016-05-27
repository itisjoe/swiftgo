# 資料庫

前面章節[儲存資訊 NSUserDefaults ](../uikit/nsuserdefaults.md)已經提過，可以使用 NSUserDefaults 來儲存少量資料。而當需要儲存大量資料時，就需要使用到資料庫了，這章會分別介紹 SQLite 以及 Core Data 。

SQLite 是一個輕量的資料庫，可以使用絕大部分的 SQL 指令，如果之前已有使用 SQL 資料庫的經驗， SQLite 其實可以很快就上手(因為操作方式幾乎一樣)。而 Core Data 的背後其實也是操作 SQLite ，但將需要操作 SQL 的指令封裝起來，讓你可以更為快速的操作資料庫，如果先前沒有 SQL 資料庫的實作經驗，你也可以選擇使用 Core Data 來快速的建立並使用資料庫。
