---
title: Swift笔记-运算符
date: 2017-06-05 09:06:54
tags: 
	- Swift 
categories: Swift 
---

## 赋值运算符 (=)

```swift
let b = 10
var a = 5
a = b
// a 现在等于 10
```


## 算术运算符 

* 加法（+）
* 减法（-）
* 乘法（*）
* 除法（/）

```swift
1 + 2       // 等于 3
5 - 3       // 等于 2
2 * 3       // 等于 6
10.0 / 2.5  // 等于 4.0
```

加法运算符也可用于String的拼接：

```swift
"hello, " + "world"  // 等于 "hello, world"
```

<!-- more -->

## 求余运算符 (%)

```swift
9 % 4    // 等于 1
```

```swift
8 % 2.5   // 等于 0.5
```

## 自增和自减 (++) (--)


* 当++前置的时候，先自増再返回。
* 当++后置的时候，先返回再自增。

```swift
var a = 0
let b = ++a // a 和 b 现在都是 1
let c = a++ // a 现在 2, 但 c 是 a 自增前的值 1
```

## 一元负号运算符

```swift
let three = 3
let minusThree = -three       // minusThree 等于 -3
let plusThree = -minusThree   // plusThree 等于 3, 或 "负负3"
```

## 一元正号运算符

一元正号（+）不做任何改变地返回操作数的值：
```swift
let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix 等于 -6
```

## 组合赋值运算符

表达式a += 2是a = a + 2的简写

```swift
var a = 1
a += 2 // a 现在是 3
```

## 比较运算符

* 等于（a == b）
* 不等于（a != b）
* 大于（a > b）
* 小于（a < b）
* 大于等于（a >= b）
* 小于等于（a <= b）

```swift
1 == 1   // true, 因为 1 等于 1
2 != 1   // true, 因为 2 不等于 1
2 > 1    // true, 因为 2 大于 1
1 < 2    // true, 因为 1 小于2
1 >= 1   // true, 因为 1 大于等于 1
2 <= 1   // false, 因为 2 并不小于等于 1
```

## 三目运算符 (a?b:c)

问题 ? 答案1 : 答案2

如果问题成立，返回答案1的结果; 如果不成立，返回答案2的结果。

## 空合运算符 (a ?? b)

空合运算符(a ?? b)将对可选类型a进行空判断，如果a包含一个值就进行解封，否则就返回一个默认值b.这个运算符有两个条件:

* 表达式a必须是Optional可选类型
* 默认值b的类型必须要和a存储值的类型保持一致

```swift
let defaultColorName = "red"
var userDefinedColorName: String?   //默认值为 nil

var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName 的值为空，所以 colorNameToUse 的值为 "red"

```

```swift
userDefinedColorName = "green"
colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName 非空，因此 colorNameToUse 的值为 "green"
```

## 区间运算符

### 闭区间运算符 （a...b）

```swift
for index in 1...5 {
    print("\(index) * 5 = \(index * 5)")
}
// 1 * 5 = 5
// 2 * 5 = 10
// 3 * 5 = 15
// 4 * 5 = 20
// 5 * 5 = 25
```

### 半开区间运算符 (a..<b)

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("第 \(i + 1) 个人叫 \(names[i])")
}
// 第 1 个人叫 Anna
// 第 2 个人叫 Alex
// 第 3 个人叫 Brian
// 第 4 个人叫 Jack
```

## 逻辑运算

### 逻辑非 (!a)
```swift
let allowedEntry = false
if !allowedEntry {
    print("ACCESS DENIED")
}
// 输出 "ACCESS DENIED"
```

### 逻辑与 (a && b)
```swift
let enteredDoorCode = true
let passedRetinaScan = false
if enteredDoorCode && passedRetinaScan {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// 输出 "ACCESS DENIED"
```

### 逻辑或 (a || b)
```swift
let hasDoorKey = false
let knowsOverridePassword = true
if hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// 输出 "Welcome!"
```

### 使用括号来明确优先级

```swift
if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// 输出 "Welcome!"
```