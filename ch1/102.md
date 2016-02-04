# 控制流程

###if
```
if var v = vvv {
}

var n = "hi, \(n1 ?? n2)"
```

###switch
```
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
```

###for-in
```
for (key, values) in dicts {

}

for n in arrs {

}
```

###for

```
//原始用法
var n = 0
for var i = 0; i < 5 ; i++ {
    n += i
}

// 與上面相同 不包含上限
for i in 0..<5 {
    n += i
}

// 包含上限
for i in 0...5 {
    n += i
}
```

###while and repeat-while
```
var n = 2
while n < 20 {
    n = n * 2
}

// 一定會先執行一次再看 while 條件
repeat {
    n = n * 2
} while n < 100

```


