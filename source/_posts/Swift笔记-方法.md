---
title: Swift笔记-方法
date: 2017-06-06 13:07:15
tags: 
	- Swift 
categories: Swift 
---

## 实例方法
在 Swift 语言中，实例方法是属于某个特定类、结构体或者枚举类型实例的方法。
实例方法提供以下方法：
- 可以访问和修改实例属性
- 提供与实例目的相关的功能

实例方法要写在它所属的类型的前后大括号({})之间。
实例方法能够隐式访问它所属类型的所有的其他实例方法和属性。
实例方法只能被它所属的类的某个特定实例调用。
实例方法不能脱离于现存的实例而被调用。

<!-- more -->

```swift
import Cocoa

class Counter {
    var count = 0
    func increment() {
        count += 1
    }
    func incrementBy(amount: Int) {
        count += amount
    }
    func reset() {
        count = 0
    }
}
// 初始计数值是0
let counter = Counter()

// 计数值现在是1
counter.increment()

// 计数值现在是6
counter.incrementBy(amount: 5)
print(counter.count)

// 计数值现在是0
counter.reset()
print(counter.count)
```

## 方法的局部参数名称和外部参数名称

```swift
import Cocoa

class multiplication {
    var count: Int = 0
    func incrementBy(first no1: Int, no2: Int) {
        count = no1 * no2
        print(count)
    }
}

let counter = multiplication()
counter.incrementBy(first: 800, no2: 3)
counter.incrementBy(first: 100, no2: 5)
counter.incrementBy(first: 15000, no2: 3)
//我们呢也可以使用下划线（_）设置第二个及后续的参数不提供一个外部名称。

```

## self 属性

类型的每一个实例都有一个隐含属性叫做self，self 完全等同于该实例本身。

```swift
import Cocoa

class calculations {
    let a: Int
    let b: Int
    let res: Int
    
    init(a: Int, b: Int) {
        self.a = a
        self.b = b
        res = a + b
        print("Self 内: \(res)")
    }
    
    func tot(c: Int) -> Int {
        return res - c
    }
    
    func result() {
        print("结果为: \(tot(c: 20))")
        print("结果为: \(tot(c: 50))")
    }
}

let pri = calculations(a: 600, b: 300)
let sum = calculations(a: 1200, b: 300)

pri.result()
sum.result()
//Self 内: 900
//Self 内: 1500
//结果为: 880
//结果为: 850
//结果为: 1480
//结果为: 1450
```

## 在实例方法中修改值类型

Swift 语言中结构体和枚举是值类型。一般情况下，值类型的属性不能在它的实例方法中被修改。

```swift
import Cocoa

struct area {
    var length = 1
    var breadth = 1
    
    func area() -> Int {
        return length * breadth
    }
    
    //使用mutating方法可以在实例中改变值类型的属性
    mutating func scaleBy(res: Int) {
        length *= res
        breadth *= res
        
        print(length)
        print(breadth)
    }
}

var val = area(length: 3, breadth: 5)
val.scaleBy(res: 3)
val.scaleBy(res: 30)
val.scaleBy(res: 300)
```

## 类型方法

类型本身调用的方法，这种方法就叫做类型方法。

声明结构体和枚举的类型方法，在方法的func关键字之前加上关键字static。类可能会用关键字class来允许子类重写父类的实现方法。

```swift
mport Cocoa

//类
class Math
{
    //类型方法
    class func abs(number: Int) -> Int
    {
        if number < 0
        {
            return (-number)
        }
        else
        {
            return number
        }
    }
}

//结构体
struct absno
{
    //类型方法
    static func abs(number: Int) -> Int
    {
        if number < 0
        {
            return (-number)
        }
        else
        {
            return number
        }
    }
}

let no = Math.abs(number: -35)
let num = absno.abs(number: -5)

print(no)
print(num)
//35
//5
```