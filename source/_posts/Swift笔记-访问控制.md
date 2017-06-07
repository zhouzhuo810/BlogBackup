---
title: Swift笔记-访问控制
date: 2017-06-07 12:29:46
tags: 
	- Swift 
categories: Swift 
---

访问控制可以限定其他源文件或模块中代码对你代码的访问级别。

Swift 为代码中的实体提供了四种不同的访问级别:public、internal、fileprivate、private。

<!-- more -->

| 访问级别 | 定义 |
|:------|:-------|
|public	|可以访问自己模块中源文件里的任何实体，别人也可以通过引入该模块来访问源文件里的所有实体。|
|internal|	可以访问自己模块中源文件里的任何实体，但是别人不能访问该模块中源文件里的实体。|
|fileprivate|	文件内私有，只能在当前源文件中使用。|
|private|	只能在类中访问，离开了这个类或者结构体的作用域外面就无法访问。|


## 语法


```swift
public class SomePublicClass {}
internal class SomeInternalClass {}
fileprivate class SomeFilePrivateClass {}
private class SomePrivateClass {}
  
public var somePublicVariable = 0
internal let someInternalConstant = 0
fileprivate func someFilePrivateFunction() {}
private func somePrivateFunction() {}
```

除非有特殊的说明，否则实体都使用默认的访问级别 internal。

## 函数类型访问权限

```swift
func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // 函数实现
}
```

## 枚举类型访问权限
枚举中成员的访问级别继承自该枚举，你不能为枚举中的成员单独申明不同的访问级别。
  
枚举 Student 被明确的申明为 public 级别，那么它的成员 Name，Mark 的访问级别同样也是 public：

```swift
public enum Student {
    case Name(String)
    case Mark(Int,Int,Int)
}
 
var studDetails = Student.Name("Swift")
var studMarks = Student.Mark(98,97,95)
 
switch studMarks {
case .Name(let studName):
    print("学生名: \(studName).")
case .Mark(let Mark1, let Mark2, let Mark3):
    print("学生成绩: \(Mark1),\(Mark2),\(Mark3)")
}
```

## 子类访问权限

子类的访问级别不得高于父类的访问级别。

```swift
public class SuperClass {
    fileprivate func show() {
        print("超类")
    }
}
 
// 访问级别不能低于超类 internal > public
internal class SubClass: SuperClass  {
    override internal func show() {
        print("子类")
    }
}
 
let sup = SuperClass()
sup.show()
 
let sub = SubClass()
sub.show()
```

## 常量、变量、属性、下标访问权限

**==常量、变量、属性不能拥有比它们的类型更高的访问级别==**。

如果常量、变量、属性、下标索引的定义类型是private级别的，那么它们必须要明确的申明访问级别为private:

```swift
private var privateInstance = SomePrivateClass()
```

## Getter 和 Setter访问权限

常量、变量、属性、下标索引的Getters和Setters的访问级别继承自它们所属成员的访问级别。

Setter的访问级别可以低于对应的Getter的访问级别，这样就可以控制变量、属性或下标索引的读写权限。

```swift
class Samplepgm {
    fileprivate var counter: Int = 0{
        willSet(newTotal){
            print("计数器: \(newTotal)")
        }
        didSet{
            if counter > oldValue {
                print("新增加数量 \(counter - oldValue)")
            }
        }
    }
}
 
let NewCounter = Samplepgm()
NewCounter.counter = 100
NewCounter.counter = 800
```

## 构造器和默认构造器访问权限

我们可以给**自定义的**初始化方法申明访问级别，但是要不高于它所属类的访问级别。

如同函数或方法参数，初始化方法参数的访问级别也不能低于初始化方法的访问级别。

默认初始化方法的访问级别与所属类型的访问级别相同。


## 协议访问权限

如果你定义了一个public访问级别的协议，那么实现该协议提供的必要函数也会是public的访问级别。

```swift
public protocol TcpProtocol {
    init(no1: Int)
}
 
public class MainClass {
    var no1: Int // local storage
    init(no1: Int) {
        self.no1 = no1 // initialization
    }
}
 
class SubClass: MainClass, TcpProtocol {
    var no2: Int
    init(no1: Int, no2 : Int) {
        self.no2 = no2
        super.init(no1:no1)
    }
    
    // Requires only one parameter for convenient method
    required override convenience init(no1: Int)  {
        self.init(no1:no1, no2:0)
    }
}
 
let res = MainClass(no1: 20)
let show = SubClass(no1: 30, no2: 50)
 
print("res is: \(res.no1)")
print("res is: \(show.no1)")
print("res is: \(show.no2)")
```

## 扩展访问权限

你可以在条件允许的情况下对类、结构体、枚举进行扩展。


## 泛型访问权限

泛型类型或泛型函数的访问级别取泛型类型、函数本身、泛型类型参数三者中的最低访问级别。

```swift
public struct TOS<T> {
    var items = [T]()
    private mutating func push(item: T) {
        items.append(item)
    }
    
    mutating func pop() -> T {
        return items.removeLast()
    }
}
 
var tos = TOS<String>()
tos.push("Swift")
print(tos.items)
 
tos.push("泛型")
print(tos.items)
 
tos.push("类型参数")
print(tos.items)
 
tos.push("类型参数名")
print(tos.items)
let deletetos = tos.pop()
```

## 类型别名

任何你定义的类型别名都会被当作不同的类型，以便于进行访问控制。一个类型别名的访问级别不可高于原类型的访问级别。

