---
title: Swift笔记-基础部分
date: 2017-06-02 16:18:33
tags: 
	- Swift 
categories: Swift 
---

## 常量和变量

1. 变量

```swift
//变量声明
var x = 1

//声明多个变量
var x = 1, y = 2

//带类型声明
var msg: String
//变量赋值
msg = "Hello"
```

2. 常量

```swift
//声明常量
let pi = 3.14159
```

<!-- more -->

## 注释

1. 单行注释

```
//xxx
```

2. 多行注释

```
/*
xxx
xxx
*/
```

## 基础数据类型

1. 整数 

（1）Int ：有符号整数

- 在32位平台上，Int和Int32长度相同。
- 在64位平台上，Int和Int64长度相同。

（2）UInt : 无符号整数

- 在32位平台上，UInt和UInt32长度相同。
- 在64位平台上，UInt和UInt64长度相同。

推荐使用Int，提高代码的可复用性。



2. 浮点数

Double ： 64位浮点数
Float ： 32位浮点数

**注意**：Double精确度很高，至少有15位数字，而Float最少只有6位数字。选择哪个类型取决于你的代码需要处理的值的范围。

3. 布尔类型

Bool

```swift
let a = true

let b = false
```


## 进制

- 一个十进制数，没有前缀
- 一个二进制数，前缀是0b
- 一个八进制数，前缀是0o
- 一个十六进制数，前缀是0x

```swift
let decimalInteger = 17 
let binaryInteger = 0b10001       // 二进制的17 
let octalInteger = 0o21           // 八进制的17 
let hexadecimalInteger = 0x11     // 十六进制的17 
```

如果一个十进制数的指数为exp，那这个数相当于基数和$10^{exp}$的乘积：
 
1.25e2 表示 $1.25 × 10^{2}$，等于 125.0。
1.25e-2 表示 $1.25 × 10^{-2}$，等于 0.0125。
 
如果一个十六进制数的指数为exp，那这个数相当于基数和$2^{exp}$的乘积：
 
0xFp2 表示 $15 × 2^{2}$，等于 60.0。
0xFp-2 表示 $15 × 2^{-2}$，等于 3.75。


## 类型转换

1. 整数转换

```swift
// 下划线用来增加可读性，不会影响值
let twoThousand: UInt16 = 2_000 
let one: UInt8 = 1 
// 将UInt8转换成UInt16再相加
let twoThousandAndOne = twoThousand + UInt16(one) 
```

2. 浮点数转换

（1）整数->浮点数

```swift
let three = 3
let t = Double(three)
```

（2）浮点数->整数

```swift
let d = 0.123
let i = Int(d) // i = 0
```

## 类型别名

```swift
typealias AudioSample = UInt16

```

## 元组

元组（tuples）把多个值组合成一个复合值。元组内的值可以使任意类型，并不要求是相同类型。


```swift
let http404Error = (404, "Not Found") 
// http404Error 的类型是 (Int, String)，值是 (404, "Not Found") 
```
你可以将一个元组的内容分解（decompose）成单独的常量和变量
```swift
let (statusCode, statusMessage) = http404Error 
println("The status code is \(statusCode)") 
// 输出 "The status code is 404" 
println("The status message is \(statusMessage)") 
// 输出 "The status message is Not Found" 
```
如果你只需要一部分元组值，分解的时候可以把要忽略的部分用下划线（_）标记
```swift
let (justTheStatusCode, _) = http404Error 
println("The status code is \(justTheStatusCode)") 
// 输出 "The status code is 404" 
```
你还可以通过下标来访问元组中的单个元素，下标从零开始
```swift
println("The status code is \(http404Error.0)") 
// 输出 "The status code is 404" 
println("The status message is \(http404Error.1)") 
// 输出 "The status message is Not Found" 
```
你可以在定义元组的时候给单个元素命名
```swift
let http200Status = (statusCode: 200, description: "OK") 
println("The status code is \(http200Status.statusCode)") 
// 输出 "The status code is 200" 
println("The status message is \(http200Status.description)") 
// 输出 "The status message is OK" 


```


## 可选 （?）

可选表示：
 
**有值，等于 x**   或者   **没有值**

"Int?"表示可选的Int

```swift
let possibleNumber = "123" 
let convertedNumber = possibleNumber.toInt() 
// convertedNumber 被推测为类型 "Int?"， 或者类型 "optional Int"
```
判断可选是否有值
```swift
if let actualNumber = possibleNumber.toInt() { 
    println("\(possibleNumber) has an integer value of \(actualNumber)") 
} else { 
    println("\(possibleNumber) could not be converted to an integer") 
} 
// 输出 "123 has an integer value of 123" 

```

### 隐式解析可选 （!）

```swift
let possibleString: String? = "An optional string." 
println(possibleString!) // 需要惊叹号来获取值 
// 输出 "An optional string." 
 
let assumedString: String! = "An implicitly unwrapped optional string." 
println(assumedString)  // 不需要感叹号 
// 输出 "An implicitly unwrapped optional string." 
```

**注意**：如果你在隐式解析可选没有值的时候尝试取值，会触发运行时错误。和你在没有值的普通可选后面加一个惊叹号一样。

## nil

可以给可选变量赋值为nil来表示它没有值

```swift
var serverResponseCode: Int? = 404 
// serverResponseCode 包含一个可选的 Int 值 404 
serverResponseCode = nil 
// serverResponseCode 现在不包含值 
```

如果你声明一个可选常量或者变量但是没有赋值，它们会自动被设置为nil：
```swift
var surveyAnswer: String? 
// surveyAnswer 被自动设置为 nil 

```
## 异常处理

throws 抛出异常

try method 抓取并执行可能抛出异常的方法

do {try ...} catch ErrorType {...处理} 处理异常

```swift
func makeASandwich() throws {
    // ...
}

do {
    try makeASandwich()
    eatASandwich()
} catch Error.OutOfCleanDishes {
    washDishes()
} catch Error.MissingIngredients(let ingredients) {
    buyGroceries(ingredients)
}
```


## 断言 （assert）

```swift
let age = -3 
assert(age >= 0, "A person's age cannot be less than zero") 
// 因为 age < 0，所以断言会触发 
```

何时使用断言
当条件可能为假时使用断言，但是最终一定要保证条件为真，这样你的代码才能继续运行。

断言的适用情景：
 
- 整数的附属脚本索引被传入一个自定义附属脚本实现，但是下标索引值可能太小或者太大。
- 需要给函数传入一个值，但是非法的值可能导致函数不能正常执行。
- 一个可选值现在是nil，但是后面的代码运行需要一个非nil值。