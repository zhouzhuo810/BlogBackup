---
title: Swift笔记-属性
date: 2017-06-06 10:35:36
tags: 
	- Swift 
categories: Swift 
---

## 存储属性

```swift
import Cocoa
  
struct Number
{
   var digits: Int
   let pi = 3.1415
}
  
var n = Number(digits: 12345)
n.digits = 67
  
print("\(n.digits)")
print("\(n.pi)")
```

<!-- more -->

存储属性可以是**变量存储属性**（用关键字var定义），也可以是**常量存储属性**（用关键字let定义）。
- 可以在定义存储属性的时候指定默认值
- 也可以在构造过程中设置或修改存储属性的值，甚至修改常量存储属性的值

```swift
import Cocoa
  
struct Number
{
   var digits: Int
   let pi = 3.1415
}
  
var n = Number(digits: 12345)
n.digits = 67
  
print("\(n.digits)")
print("\(n.pi)")
//67
//3.1415
```

//构造方法给常量赋值
```swift
import Cocoa
  
struct Number
{
    var digits: Int
    let numbers: Double
    init(digits: Int, numbers : Double) {
        self.digits = digits;
        self.numbers = numbers;
    }
}
  
var n = Number(digits: 12345, numbers: 3.14)
n.digits = 67
  
print("\(n.digits)")
print("\(n.numbers)")
//67
//3.14
```

### 延迟存储属性
延迟存储属性是指当第一次被调用的时候才会计算其初始值的属性。

延迟存储属性一般用于：
- 延迟对象的创建。
- 当属性的值依赖于其他未知类

```swift
import Cocoa
  
class sample {
    lazy var no = number() // `var` 关键字是必须的
}
  
class number {
    var name = "Runoob Swift 教程"
}
  
var firstsample = sample()
print(firstsample.no.name)
//Runoob Swift 教程
```



## 计算属性
计算属性不直接存储值，而是提供一个 getter 来获取值，一个可选的 setter 来间接设置其他属性或变量的值。
```swift
import Cocoa
  
class sample {
    var no1 = 0.0, no2 = 0.0
    var length = 300.0, breadth = 150.0
    
    var middle: (Double, Double) {
        get{
            return (length / 2, breadth / 2)
        }
        set(axis){
            no1 = axis.0 - (length / 2)
            no2 = axis.1 - (breadth / 2)
        }
    }
}
  
var result = sample()
print(result.middle)
result.middle = (0.0, 10.0)
  
print(result.no1)
print(result.no2)
//(150.0, 75.0)
//-150.0
//-65.0
```
### 只读计算属性
只有 getter 没有 setter 的计算属性就是只读计算属性。
```swift
import Cocoa
  
class film {
    var head = ""
    var duration = 0.0
    var metaInfo: [String:String] {
        return [
            "head": self.head,
            "duration":"\(self.duration)"
        ]
    }
}
  
var movie = film()
movie.head = "Swift 属性"
movie.duration = 3.09
  
print(movie.metaInfo["head"]!)
print(movie.metaInfo["duration"]!)
  
//Swift 属性
//3.09
```

## 属性观察器

属性观察器监控和响应属性值的变化，每次属性被设置值的时候都会调用属性观察器，甚至新的值和现在的值相同的时候也不例外。

可以为属性添加如下的一个或全部观察器：

- willSet在设置新的值之前调用
- didSet在新的值被设置之后立即调用
- willSet和didSet观察器在属性初始化过程中不会被调用

```swift
import Cocoa
  
class Samplepgm {
    var counter: Int = 0{
        willSet(newTotal){
            print("计数器: \(newTotal)")
        }
        didSet{
            if counter > oldValue {
                print("新增数 \(counter - oldValue)")
            }
        }
    }
}
let NewCounter = Samplepgm()
NewCounter.counter = 100
NewCounter.counter = 800
//计数器: 100
//新增数 100
//计数器: 800
//新增数 700
```

## 类型属性

类型属性是作为类型定义的一部分写在类型最外层的花括号（{}）内。

使用关键字 **static** 来定义值类型的类型属性，关键字 **class** 来为类定义类型属性。

```swift
struct Structname {
   static var storedTypeProperty = " "
   static var computedTypeProperty: Int {
      // 这里返回一个 Int 值
   }
}
  
enum Enumname {
   static var storedTypeProperty = " "
   static var computedTypeProperty: Int {
      // 这里返回一个 Int 值
   }
}
  
class Classname {
   class var computedTypeProperty: Int {
      // 这里返回一个 Int 值
   }
}
```

### 获取和设置类型属性的值

类型属性是通过类型本身来获取和设置，而不是通过实例。

```swift
import Cocoa
  
struct StudMarks {
   static let markCount = 97
   static var totalCount = 0
   var InternalMarks: Int = 0 {
      didSet {
         if InternalMarks > StudMarks.markCount {
            InternalMarks = StudMarks.markCount
         }
         if InternalMarks > StudMarks.totalCount {
            //类型属性赋值
            StudMarks.totalCount = InternalMarks
         }
      }
   }
}
  
var stud1Mark1 = StudMarks()
var stud1Mark2 = StudMarks()
  
stud1Mark1.InternalMarks = 98
print(stud1Mark1.InternalMarks) 
  
stud1Mark2.InternalMarks = 87
print(stud1Mark2.InternalMarks)
//97
//87
```