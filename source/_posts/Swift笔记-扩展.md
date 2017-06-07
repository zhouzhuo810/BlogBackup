---
title: Swift笔记-扩展
date: 2017-06-06 16:53:57
tags: 
	- Swift 
categories: Swift 
---

扩展就是向一个已有的类、结构体或枚举类型添加新功能。
扩展可以对一个类型添加新的功能，但是不能重写已有的功能。

Swift 中的扩展可以：
- 添加计算型属性和计算型静态属性
- 定义实例方法和类型方法
- 提供新的构造器
- 定义下标
- 定义和使用新的嵌套类型
- 使一个已有类型符合某个协议

<!-- more -->

## 语法


扩展声明使用关键字 extension：

```swift
extension SomeType {
    // 加到SomeType的新功能写到这里
}
```

```swift
extension SomeType: SomeProtocol, AnotherProctocol {
    // 协议实现写到这里
}
```


## 计算型属性

扩展可以向已有类型添加计算型实例属性和计算型类型属性。


```swift
extension Int {
   var add: Int {return self + 100 }
   var sub: Int { return self - 10 }
   var mul: Int { return self * 10 }
   var div: Int { return self / 5 }
}
    
let addition = 3.add
print("加法运算后的值：\(addition)")
    
let subtraction = 120.sub
print("减法运算后的值：\(subtraction)")
    
let multiplication = 39.mul
print("乘法运算后的值：\(multiplication)")
    
let division = 55.div
print("除法运算后的值: \(division)")
    
let mix = 30.add + 34.sub
print("混合运算结果：\(mix)")
```


## 构造器

扩展可以向已有类型添加新的构造器。


```swift
struct sum {
    var num1 = 100, num2 = 200
}
    
struct diff {
    var no1 = 200, no2 = 100
}
    
struct mult {
    var a = sum()
    var b = diff()
}
    
extension mult {
    init(x: sum, y: diff) {
        _ = x.num1 + x.num2
        _ = y.no1 + y.no2
    }
}
    
let a = sum(num1: 100, num2: 200)
let b = diff(no1: 200, no2: 100)
let getMult = mult(x: a, y: b)
    
print("getMult sum\(getMult.a.num1, getMult.a.num2)")
print("getMult diff\(getMult.b.no1, getMult.b.no2)")   
```


## 方法

扩展可以向已有类型添加新的实例方法和类型方法。


```swift
extension Int {
   func topics(summation: () -> ()) {
      for _ in 0..<self {
         summation() 
      }
   }
}  

4.topics({
   print("扩展模块内")       
})    
    
3.topics({
   print("内型转换模块内")       
})  
```
  

这个topics方法使用了一个() -> ()类型的单参数，表明函数没有参数而且没有返回值。

## 可变实例方法

通过扩展添加的实例方法也可以修改该实例本身。

  

```swift
extension Double {
   mutating func square() {
      let pi = 3.1415
      self = pi * self * self
   }
}
    
var Trial1 = 3.3
Trial1.square()
print("圆的面积为: \(Trial1)")
    
var Trial2 = 5.8
Trial2.square()
print("圆的面积为: \(Trial2)")
    
var Trial3 = 120.3
Trial3.square()
print("圆的面积为: \(Trial3)")  
```
  

## 下标

扩展可以向一个已有类型添加新下标。

  

```swift
extension Int {
   subscript(var multtable: Int) -> Int {
      var no1 = 1
      while multtable > 0 {
         no1 *= 10
         --multtable
      }
      return (self / no1) % 10
   }
}
    
print(12[0])  //2
print(7869[1]) //6
print(786543[2]) //5      
```
  

## 嵌套类型

扩展可以向已有的类、结构体和枚举添加新的嵌套类型：
  

```swift
extension Int {
   enum calc
   {
      case add
      case sub
      case mult
      case div
      case anything
   }
  
   var print: calc {
      switch self
      {
         case 0:
            return .add
         case 1:
            return .sub
         case 2:
            return .mult
         case 3:
            return .div
         default:
            return .anything
       }
   }
}
    
func result(numb: [Int]) {
   for i in numb {
      switch i.print {
         case .add:
            print(" 10 ")
          case .sub:
            print(" 20 ")
         case .mult:
         print(" 30 ")
         case .div:
         print(" 40 ")
         default:
         print(" 50 ")

      }
   }
}
    
result([0, 1, 2, 3, 4, 7])
```
  
