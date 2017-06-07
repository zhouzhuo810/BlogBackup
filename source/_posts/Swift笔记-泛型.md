---
title: Swift笔记-泛型
date: 2017-06-07 12:28:41
tags: 
	- Swift 
categories: Swift 
---

Swift 提供了泛型让你写出灵活且可重用的函数和类型。
Swift 标准库是通过泛型代码构建出来的。
Swift 的数组和字典类型都是泛型集。

泛型使用了占位类型名（在这里用字母 T 来表示）来代替实际类型名（例如 Int、String 或 Double）。

<!-- more -->

## 语法

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```
## 实例

```swift
// 定义一个交换两个变量的函数
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
 
var numb1 = 100
var numb2 = 200
 
print("交换前数据:  \(numb1) 和 \(numb2)")
swapTwoValues(&numb1, &numb2)
print("交换后数据: \(numb1) 和 \(numb2)")
 
var str1 = "A"
var str2 = "B"
 
print("交换前数据:  \(str1) 和 \(str2)")
swapTwoValues(&str1, &str2)
print("交换后数据: \(str1) 和 \(str2)")
```

## 泛型类型

Swift 允许你定义你自己的泛型类型。

```swift
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
 
var stackOfStrings = Stack<String>()
print("字符串元素入栈: ")
stackOfStrings.push("google")
stackOfStrings.push("runoob")
print(stackOfStrings.items);
 
let deletetos = stackOfStrings.pop()
print("出栈元素: " + deletetos)
 
var stackOfInts = Stack<Int>()
print("整数元素入栈: ")
stackOfInts.push(1)
stackOfInts.push(2)
print(stackOfInts.items);

```

## 扩展泛型类型

当你扩展一个泛型类型的时候（使用 extension 关键字），你并不需要在扩展的定义中提供类型参数列表。

```swift
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
 
extension Stack {
    var topItem: Element? {
       return items.isEmpty ? nil : items[items.count - 1]
    }
}
 
var stackOfStrings = Stack<String>()
print("字符串元素入栈: ")
stackOfStrings.push("google")
stackOfStrings.push("runoob")
 
if let topItem = stackOfStrings.topItem {
    print("栈中的顶部元素是：\(topItem).")
}
 
print(stackOfStrings.items)
```

## 类型约束
类型约束指定了一个必须继承自指定类的类型参数，或者遵循一个特定的协议或协议构成。

### 类型约束语法

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // 这里是泛型函数的函数体部分
}
```

上面这个函数有两个类型参数。第一个类型参数 T，有一个要求 T 必须是 SomeClass 子类的类型约束；第二个类型参数 U，有一个要求 U 必须符合 SomeProtocol 协议的类型约束。


### 实例

```swift
// 非泛型函数，查找指定字符串在数组中的索引
func findIndex(ofString valueToFind: String, in array: [String]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            // 找到返回索引值
            return index
        }
    }
    return nil
}
 
 
let strings = ["google", "weibo", "taobao", "runoob", "facebook"]
if let foundIndex = findIndex(ofString: "runoob", in: strings) {
    print("runoob 的索引为 \(foundIndex)")
}
```

## 关联类

Swift 中使用 associatedtype 关键字来设置关联类型实例。

```swift
// Container 协议
protocol Container {
    associatedtype ItemType
    // 添加一个新元素到容器里
    mutating func append(_ item: ItemType)
    // 获取容器中元素的数
    var count: Int { get }
    // 通过索引值类型为 Int 的下标检索到容器中的每一个元素
    subscript(i: Int) -> ItemType { get }
}

// Stack 结构体遵从 Container 协议
struct Stack<Element>: Container {
    // Stack<Element> 的原始实现部分
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
    // Container 协议的实现部分
    mutating func append(_ item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}

var tos = Stack<String>()
tos.push("google")
tos.push("runoob")
tos.push("taobao")
// 元素列表
print(tos.items)
// 元素个数
print( tos.count)
```

## Where 语句

类型约束能够确保类型符合泛型函数或类的定义约束。

你可以在参数列表中通过where语句定义参数的约束。

### 实例
下面的例子定义了一个名为allItemsMatch的泛型函数，用来检查两个Container实例是否包含相同顺序的相同元素。
如果所有的元素能够匹配，那么返回 true，反之则返回 false。

```swift
// Container 协议
protocol Container {
    associatedtype ItemType
    // 添加一个新元素到容器里
    mutating func append(_ item: ItemType)
    // 获取容器中元素的数
    var count: Int { get }
    // 通过索引值类型为 Int 的下标检索到容器中的每一个元素
    subscript(i: Int) -> ItemType { get }
}
  
// // 遵循Container协议的泛型TOS类型
struct Stack<Element>: Container {
    // Stack<Element> 的原始实现部分
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
    // Container 协议的实现部分
    mutating func append(_ item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}
// 扩展，将 Array 当作 Container 来使用
extension Array: Container {}
  
func allItemsMatch<C1: Container, C2: Container>
    (_ someContainer: C1, _ anotherContainer: C2) -> Bool
    where C1.ItemType == C2.ItemType, C1.ItemType: Equatable {
        
        // 检查两个容器含有相同数量的元素
        if someContainer.count != anotherContainer.count {
            return false
        }
        
        // 检查每一对元素是否相等
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
            }
        }
        
        // 所有元素都匹配，返回 true
        return true
}
var tos = Stack<String>()
tos.push("google")
tos.push("runoob")
tos.push("taobao")
   
var aos = ["google", "runoob", "taobao"]
   
if allItemsMatch(tos, aos) {
    print("匹配所有元素")
} else {
    print("元素不匹配")
}
  
//匹配所有元素
```