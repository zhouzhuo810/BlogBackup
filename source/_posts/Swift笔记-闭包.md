---
title: Swift笔记-闭包
date: 2017-06-05 17:27:37
tags: 
	- Swift 
categories: Swift 
---

## sorted 方法

排序闭包函数类型需为(String, String) -> Bool。
```swift
import Cocoa
  
let names = ["AT", "AE", "D", "S", "BE"]
  
// 使用普通函数(或内嵌函数)提供排序功能,闭包函数类型需为(String, String) -> Bool。
func backwards(s1: String, s2: String) -> Bool {
    return s1 > s2
}
var reversed = names.sorted(by: backwards)
  
print(reversed)
```

<!-- more -->

## 闭包表达式语法
```swift
{ (parameters) -> returnType in
    statements
}
```
下面的例子展示了之前backwards(_:_:)函数对应的闭包表达式版本的代码：
```swift
var reverses = names.sorted(by: { (s1 , s2) -> Bool in
    return s1 > s2
})
```

### 根据上下文推断类型
```swift
reversed = names.sorted(by: { s1, s2 in return s1 > s2 } )
```

### 单表达式闭包隐式返回
```swift
reversed = names.sorted(by: { s1, s2 in s1 > s2 } )
```
在这个例子中，sort(_:)方法的第二个参数函数类型明确了闭包必须返回一个Bool类型值。因为闭包函数体只包含了一个单一表达式（s1 > s2），该表达式返回Bool类型值，因此这里没有歧义，return关键字可以省略。

### 参数名称缩写

Swift 自动为内联闭包提供了参数名称缩写功能，您可以直接通过$0，$1，$2来顺序调用闭包的参数，以此类推。

```swift
import Cocoa
  
let names = ["AT", "AE", "D", "S", "BE"]
  
var reversed = names.sorted( by: { $0 > $1 } )
print(reversed)
```
在这个例子中，$0和$1表示闭包中第一个和第二个String类型的参数。


### 运算符函数

```swift
import Cocoa
  
let names = ["AT", "AE", "D", "S", "BE"]
  
var reversed = names.sorted(by: >)
print(reversed)
```
 
## 尾随闭包
尾随闭包是一个书写在函数括号之后的闭包表达式，函数支持将其作为最后一个参数调用。
```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // 函数体部分
}
  
// 以下是不使用尾随闭包进行函数调用
someFunctionThatTakesAClosure({
    // 闭包主体部分
})
  
// 以下是使用尾随闭包进行函数调用
someFunctionThatTakesAClosure() {
    // 闭包主体部分
}
```

如果函数只需要闭包表达式一个参数，当您使用尾随闭包时，您甚至可以把()省略掉：
```swift
import Cocoa
  
let names = ["AT", "AE", "D", "S", "BE"]
  
//尾随闭包
var reversed = names.sorted() { $0 > $1 }
print(reversed)
```
**注意**： 如果函数只需要闭包表达式一个参数，当您使用尾随闭包时，您甚至可以把()省略掉。
```swift
reversed = names.sorted { $0 > $1 }
```