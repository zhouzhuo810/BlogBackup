---
title: Swift笔记-字符串和字符
date: 2017-06-05 09:50:34
tags: 
	- Swift
categories: Swift
---

## 初始化空字符串

```swift
var emptyString = ""               // 空字符串字面量
var anotherEmptyString = String()  // 初始化方法
// 两个字符串均为空并等价。
```

判断该字符串是否为空
```swift
if emptyString.isEmpty {
    print("Nothing to see here")
}
// 打印输出："Nothing to see here"
```

<!-- more -->

## 字符串可变性

```swift
var variableString = "Horse"
variableString += " and carriage"
// variableString 现在为 "Horse and carriage"

let constantString = "Highlander"
constantString += " and another Highlander"
// 这会报告一个编译错误 (compile-time error) - 常量字符串不可以被修改。
```

## 使用字符

### 遍历字符串中的字符
```swift
for character in "Dog!🐶".characters {
    print(character)
}
// D
// o
// g
// !
// 🐶
```
### 声明字符常量
```swift
let exclamationMark: Character = "!"
```
### 字符数组转字符串
```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString)
// 打印输出："Cat!🐱"
```

## 连接字符串和字符

### +

```swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2
// welcome 现在等于 "hello there"
```

### +=

```swift
var instruction = "look over"
instruction += string2
// instruction 现在等于 "look over there"
```

### append()

```swift
let exclamationMark: Character = "!"
welcome.append(exclamationMark)
// welcome 现在等于 "hello there!"
```

## 字符串插值 

```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message is "3 times 2.5 is 7.5"
```

## 计算字符数量 (str.count)

```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.characters.count)")
// 打印输出 "the number of characters in cafe is 4"
```

## 访问和修改字符串

### 字符串索引

```swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.endIndex.predecessor()]
// !
greeting[greeting.startIndex.successor()]
// u
let index = greeting.startIndex.advancedBy(7)
greeting[index]
// a
```

```swift
for index in greeting.characters.indices {
   print("\(greeting[index]) ", terminator: " ")
}
// 打印输出 "G u t e n   T a g !"
```

### 插入和删除()

调用insert(_:atIndex:)方法可以在一个字符串的指定索引插入一个字符。
```swift
var welcome = "hello"
welcome.insert("!", atIndex: welcome.endIndex)
// welcome now 现在等于 "hello!"
```
调用insertContentsOf(_:at:)方法可以在一个字符串的指定索引插入一个字符串。
```swift
welcome.insertContentsOf(" there".characters, at: welcome.endIndex.predecessor())
// welcome 现在等于 "hello there!"
```
调用removeAtIndex(_:)方法可以在一个字符串的指定索引删除一个字符。
```swift
welcome.removeAtIndex(welcome.endIndex.predecessor())
// welcome 现在等于 "hello there"
```

调用removeRange(_:)方法可以在一个字符串的指定索引删除一个子字符串。
```swift
let range = welcome.endIndex.advancedBy(-6)..<welcome.endIndex
welcome.removeRange(range)
// welcome 现在等于 "hello"
```

## 比较字符串

### 字符串/字符相等 (== 和 !=)

```swift
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    print("These two strings are considered equal")
}
// 打印输出 "These two strings are considered equal"
```

### 前缀/后缀相等

通过调用字符串的hasPrefix(_:)/hasSuffix(_:)方法来检查字符串是否拥有特定前缀/后缀，两个方法均接收一个String类型的参数，并返回一个布尔值。

```swift
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]
```

前缀判断
```swift
var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        ++act1SceneCount
    }
}
print("There are \(act1SceneCount) scenes in Act 1")
// 打印输出 "There are 5 scenes in Act 1"
```
后缀判断
```swift
var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        ++mansionCount
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        ++cellCount
    }
}
print("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// 打印输出 "6 mansion scenes; 2 cell scenes"
```