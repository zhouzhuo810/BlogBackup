---
title: Swift笔记-类
date: 2017-06-06 09:05:10
tags: 
	- Swift 
categories: Swift 
---

## 语法


### 类定义
```swift
class student {
    var studname: String
    var mark: Int
    var mark2: Int
}
```
<!-- more -->

### 实例化类

```swift
let studrecord = student()
```

## 实例

```swift
import Cocoa

class MarksStruct {
    var mark: Int
    init(mark: Int) {
        self.mark = mark
    }
}

class studentMarks {
    var mark = 300
}
let marks = studentMarks()
print("成绩为 \(marks.mark)")
//成绩为 300
```

## 作为引用类型访问类属性

类的属性可以通过 . 来访问。格式为：实例化类名.属性名：

```swift
import Cocoa

class MarksStruct {
   var mark: Int
   init(mark: Int) {
      self.mark = mark
   }
}

class studentMarks {
   var mark1 = 300
   var mark2 = 400
   var mark3 = 900
}
let marks = studentMarks()
print("Mark1 is \(marks.mark1)")
print("Mark2 is \(marks.mark2)")
print("Mark3 is \(marks.mark3)")
//Mark1 is 300
//Mark2 is 400
//Mark3 is 900
```

## 恒等运算符

| 恒等运算符|不恒等运算符|
|:--------|:---------|
|运算符为：===|	运算符为：!==|
|如果两个常量或者变量引用同一个类实例则返回 true|	如果两个常量或者变量引用不同一个类实例则返回 true|  

```swift
import Cocoa

class SampleClass: Equatable {
    let myProperty: String
    init(s: String) {
        myProperty = s
    }
}
func ==(lhs: SampleClass, rhs: SampleClass) -> Bool {
    return lhs.myProperty == rhs.myProperty
}

let spClass1 = SampleClass(s: "Hello")
let spClass2 = SampleClass(s: "Hello")

if spClass1 === spClass2 {// false
    print("引用相同的类实例 \(spClass1)")
}

if spClass1 !== spClass2 {// true
    print("引用不相同的类实例 \(spClass2)")
}
//引用不相同的类实例 SampleClass
```