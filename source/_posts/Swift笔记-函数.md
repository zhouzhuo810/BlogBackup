---
title: Swift笔记-函数
date: 2017-06-05 15:29:35
tags: 
	- Swift 
categories: Swift 
---



## 函数的定义与调用


```swift
func sayHelloAgain(personName: String) -> String {
    return "Hello again, " + personName + "!"
}
  
print(sayHelloAgain(personName: "Anna"))
// prints "Hello again, Anna!"
```
## 函数参数与返回值

### 无参函数

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
// prints "hello, world"
```

<!-- more -->

### 多参数函数

```swift
func sayHello(personName: String, alreadyGreeted: Bool) -> String {
    if alreadyGreeted {
        return sayHelloAgain(personName: personName)
    } else {
        return sayHello(personName: personName, alreadyGreeted: <#Bool#>)
    }
}
print(sayHello(personName: "Tim", alreadyGreeted: true))
// prints "Hello again, Tim!"
```
### 无返回值函数

```swift
func sayGoodbye(personName: String) {
    print("Goodbye, \(personName)!")
}
sayGoodbye(personName: "Dave")
// prints "Goodbye, Dave!"
```
### 多重返回值函数

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```
### 可选元组返回类型

为了安全地处理这个“空数组”问题，将minMax(_:)函数改写为使用可选元组返回类型，并且当数组为空时返回nil：
```swift
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

### 函数参数名称

函数参数都有一个外部参数名（external parameter name）和一个局部参数名（local parameter name）。外部参数名用于在函数调用时标注传递给函数的参数，局部参数名在函数的实现内部使用。
```swift
func someFunction(firstParameterName: Int, secondParameterName: Int) {
    // function body goes here
    // firstParameterName and secondParameterName refer to
    // the argument values for the first and second parameters
}
someFunction(firstParameterName: 1, secondParameterName: 2)
```

### 指定外部参数名

```swift
func someFunction(externalParameterName localParameterName: Int) {
    // function body goes here, and can use localParameterName
    // to refer to the argument value for that parameter
}
```


```swift
func sayHello(to person: String, and anotherPerson: String) -> String {
    return "Hello \(person) and \(anotherPerson)!"
}
print(sayHello(to: "Bill", and: "Ted"))
// prints "Hello Bill and Ted!"
```
为每个参数指定外部参数名后，在你调用sayHello(to:and:)函数时两个外部参数名都必须写出来。

### 忽略外部参数名

如果你不想为第二个及后续的参数设置外部参数名，用一个下划线（_）代替一个明确的参数名。
```swift
func someFunction(firstParameterName: Int, _ secondParameterName: Int) {
    // function body goes here
    // firstParameterName and secondParameterName refer to
    // the argument values for the first and second parameters
}
someFunction(1, 2)
```

### 默认参数值

你可以在函数体中为每个参数定义默认值（Deafult Values）。当默认值被定义后，调用这个函数时可以忽略这个参数。
```swift
func someFunction(_ parameterWithDefault: Int = 12) {
    // function body goes here
    // if no arguments are passed to the function call,
    // value of parameterWithDefault is 12
}
someFunction(6) // parameterWithDefault is 6
someFunction() // parameterWithDefault is 12
```

### 可变参数

```swift
func arithmeticMean(numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers
arithmeticMean(3, 8.25, 18.75)
// returns 10.0, which is the arithmetic mean of these three numbers
```
**注意**:一个函数最多只能有一个可变参数。

### 输入输出参数


```swift
func swapTwoInts(a:inout Int, _ b:inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```
调用参数需要添加&

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// prints "someInt is now 107, and anotherInt is now 3"
```
从上面这个例子中，我们可以看到 someInt 和 anotherInt 的原始值在 swapTwoInts(_:_:) 函数中被修改，尽管它们的定义在函数体外。

## 函数类型

每个函数都有种特定的函数类型，由函数的参数类型和返回类型组成。

## 嵌套函数


```swift
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backwards ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(backwards: currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```
