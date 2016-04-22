# 系統關鍵字

Swift 系統內建的關鍵字有四種類型，依序如下：

#### Declarations

associatedtype, class, deinit, enum, extension, func, import, init, inout, internal, let, operator, private, protocol, public, static, struct, subscript, typealias, var.

#### Statements

break, case, continue, default, defer, do, else, fallthrough, for, guard, if, in, repeat, return, switch, where, while.

#### Expressions and types

as, catch, dynamicType, false, is, nil, rethrows, super, self, Self, throw, throws, true, try, #column, #file, #function, #line.

#### Particular contexts

associativity, convenience, dynamic, didSet, final, get, infix, indirect, lazy, left, mutating, none, nonmutating, optional, override, postfix, precedence, prefix, Protocol, required, right, set, Type, unowned, weak, willSet.


#### 命名

不建議使用系統內建的關鍵字來命名變數、常數、函式、類別及其他自定義的型別，如果真的一定要使用的話，必須以一對反單引號` ` `把名稱包起來，如下：

```swift
// 無法這樣命名 這行會報錯誤
let class = 20

// 必須以一對反單引號 ` 包起名稱
var `class` = 12

// 使用也必須加上反單引號
print("It is \(`class`)")

```


