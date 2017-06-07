---
title: Swift笔记-继承
date: 2017-06-06 13:41:14
tags: 
	- Swift 
categories: Swift 
---

在 Swift 中，类可以调用和访问超类的方法，属性和下标脚本，并且可以重写它们。

## 基类

没有继承其它类的类，称之为基类（Base Class）。


```swift
class StudDetails {
    var stname: String!
    var mark1: Int!
    var mark2: Int!
    var mark3: Int!
    init(stname: String, mark1: Int, mark2: Int, mark3: Int) {
        self.stname = stname
        self.mark1 = mark1
        self.mark2 = mark2
        self.mark3 = mark3
    }
}
let stname = "swift"
let mark1 = 98
let mark2 = 89
let mark3 = 76
  
let sds = StudDetails(stname:stname, mark1:mark1, mark2:mark2, mark3:mark3);
  
print(sds.stname)
print(sds.mark1)
print(sds.mark2)
print(sds.mark3)
//swift
//98
//89
//76
```

<!-- more -->

## 子类
子类指的是在一个已有类的基础上创建一个新的类。

```swift
class SomeClass: SomeSuperclass {
    // 类的定义
}
```

### 实例

```swift
class StudDetails
{
    var mark1: Int;
    var mark2: Int;
  
    init(stm1:Int, results stm2:Int)
    {
        mark1 = stm1;
        mark2 = stm2;
    }
  
    func show()
    {
        print("Mark1:\(self.mark1), Mark2:\(self.mark2)")
    }
}

class Tom : StudDetails
{
    init()
    {
        super.init(stm1: 93, results: 89)
    }
}
  
let tom = Tom()
tom.show()
  
//Mark1:93, Mark2:89
```

## 重写

子类可以通过继承来的实例方法，类方法，实例属性，或下标脚本来实现自己的定制功能，我们把这种行为叫重写（overriding）。


|重写|	访问方法，属性，下标脚本|
|:---|:---|
|方法|	super.somemethod()|
|属性|	super.someProperty()|
|下标脚本|	super[someIndex]|

### 重写方法和属性

#### 重写方法

```swift
class SuperClass {
    func show() {
        print("这是超类 SuperClass")
    }
}
  
class SubClass: SuperClass  {
    override func show() {
        print("这是子类 SubClass")
    }
}
  
let superClass = SuperClass()
superClass.show()
  
let subClass = SubClass()
subClass.show()
  
//这是超类 SuperClass
//这是子类 SubClass
```

#### 重写属性

注意点：
- 如果你在重写属性中提供了 setter，那么你也一定要提供 getter。
- 如果你不想在重写版本中的 getter 里修改继承来的属性值，你可以直接通过super.someProperty来返回继承来的值，其中someProperty是你要重写的属性的名字。

```swift
class Circle {
    var radius = 12.5
    var area: String {
        return "矩形半径 \(radius) "
    }
}
  
// 继承超类 Circle
class Rectangle: Circle {
    var print = 7
    override var area: String {
        return super.area + " ，但现在被重写为 \(print)"
    }
}
  
let rect = Rectangle()
rect.radius = 25.0
rect.print = 3
print("Radius \(rect.area)")
  
//Radius 矩形半径 25.0  ，但现在被重写为 3
```

### 写属性观察器

```swift
class Circle {
    var radius = 12.5
    var area: String {
        return "矩形半径为 \(radius) "
    }
}
  
class Rectangle: Circle {
    var print = 7
    override var area: String {
        return super.area + " ，但现在被重写为 \(print)"
    }
}
  
  
let rect = Rectangle()
rect.radius = 25.0
rect.print = 3
print("半径: \(rect.area)")
  
class Square: Rectangle {
    override var radius: Double {
        //重写didSet
        didSet {
            print = Int(radius/5.0)+1
        }
    }
}
  
  
let sq = Square()
sq.radius = 100.0
  
print("半径: \(sq.area)")
```

### 防止重写

我们可以使用 final 关键字防止它们被重写

```swift
final class Circle {
    final var radius = 12.5
    var area: String {
        return "矩形半径为 \(radius) "
    }
}
```