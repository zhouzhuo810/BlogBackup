---
title: Swift笔记-集合类型
date: 2017-06-05 11:01:53
tags: 
	- Swift 
categories: Swift 
---

Swift 语言提供Arrays、Sets和Dictionaries三种基本的集合类型用来存储集合数据。

## 数组

### 创建一个空数组

```swift
var someInts = [Int]()
print("someInts is of type [Int] with \(someInts.count) items.")
// 打印 "someInts is of type [Int] with 0 items."
```
### 创建一个带有默认值的数组

```swift
var threeDoubles = [Double](count: 3, repeatedValue:0.0)
// threeDoubles 是一种 [Double] 数组，等价于 [0.0, 0.0, 0.0]
```

<!-- more -->


### 通过两个数组相加创建一个数组

```swift
var anotherThreeDoubles = Array(count: 3, repeatedValue: 2.5)
// anotherThreeDoubles 被推断为 [Double]，等价于 [2.5, 2.5, 2.5]

var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles 被推断为 [Double]，等价于 [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```

### 用字面量构造数组

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList 已经被构造并且拥有两个初始项。
```
或
```swift
var shoppingList = ["Eggs", "Milk"]
```
因为所有字面量中的值都是相同的类型，Swift 可以推断出[String]是shoppinglist中变量的正确类型。

### 访问和修改数组

可以使用数组的只读属性count来获取数组中的数据项数量：
```swift
print("The shopping list contains \(shoppingList.count) items.")
// 输出 "The shopping list contains 2 items."（这个数组有2个项）
```
使用布尔值属性isEmpty作为检查count属性的值是否为 0 的捷径：
```swift
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list is not empty.")
}
// 打印 "The shopping list is not empty."（shoppinglist 不是空的）
```
也可以使用append(_:)方法在数组后面添加新的数据项：
```swift
shoppingList.append("Flour")
// shoppingList 现在有3个数据项，有人在摊煎饼
```
使用加法赋值运算符（+=）也可以直接在数组后面添加一个或多个拥有相同类型的数据项：
```swift
shoppingList += ["Baking Powder"]
// shoppingList 现在有四项了
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList 现在有七项了
```

可以直接使用下标语法来获取数组中的数据项

```swift
var firstItem = shoppingList[0]
// 第一项是 "Eggs"
```

还可以利用下标来一次改变一系列数据值，即使新数据和原有数据的数量是不一样的。下面的例子把"Chocolate Spread"，"Cheese"，和"Butter"替换为"Bananas"和 "Apples"：
```swift
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList 现在有6项
```

调用数组的insert(_:atIndex:)方法来在某个具体索引值之前添加数据项：
```swift
shoppingList.insert("Maple Syrup", atIndex: 0)
// shoppingList 现在有7项
// "Maple Syrup" 现在是这个列表中的第一项
```
类似的我们可以使用removeAtIndex(_:)方法来移除数组中的某一项。
```swift
let mapleSyrup = shoppingList.removeAtIndex(0)
// 索引值为0的数据项被移除
// shoppingList 现在只有6项，而且不包括 Maple Syrup
// mapleSyrup 常量的值等于被移除数据项的值 "Maple Syrup"
```
如果我们只想把数组中的最后一项移除，可以使用removeLast()方法
```swift
let apples = shoppingList.removeLast()
// 数组的最后一项被移除了
// shoppingList 现在只有5项，不包括 cheese
// apples 常量的值现在等于 "Apples" 字符串
```
### 数组的遍历
我们可以使用for-in循环来遍历所有数组中的数据项：
```swift
for item in shoppingList {
    print(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```
如果我们同时需要每个数据项的值和索引值，可以使用enumerate()方法来进行数组遍历。
```swift
for (index, value) in shoppingList.enumerate() {
    print("Item \(String(index + 1)): \(value)")
}
// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```


## 集合

### 创建和构造一个空的集合

```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// 打印 "letters is of type Set<Character> with 0 items."
```

### 用数组字面量创建集合

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
// favoriteGenres 被构造成含有三个初始值的集合
```
### 访问和修改一个集合

为了找出一个Set中元素的数量，可以使用其只读属性count：
```swift
print("I have \(favoriteGenres.count) favorite music genres.")
// 打印 "I have 3 favorite music genres."
```
使用布尔属性isEmpty作为一个缩写形式去检查count属性是否为0：
```swift
if favoriteGenres.isEmpty {
    print("As far as music goes, I'm not picky.")
} else {
    print("I have particular music preferences.")
}
// 打印 "I have particular music preferences."
```
你可以通过调用Set的insert(_:)方法来添加一个新元素：
```swift
favoriteGenres.insert("Jazz")
// favoriteGenres 现在包含4个元素
```
你可以通过调用Set的remove(_:)方法去删除一个元素，如果该值是该Set的一个元素则删除该元素并且返回被删除的元素值，否则如果该Set不包含该值，则返回nil
```swift
if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}
// 打印 "Rock? I'm over it."
```
使用contains(_:)方法去检查Set中是否包含一个特定的值：
```swift
if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// 打印 "It's too funky in here."
```
### 遍历一个集合

```swift
for genre in favoriteGenres {
    print("\(genre)")
}
// Classical
// Jazz
// Hip hop
```

```swift
for genre in favoriteGenres.sort() {
    print("\(genre)")
}
// prints "Classical"
// prints "Hip hop"
// prints "Jazz
```

### 基本集合操作

![](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/setVennDiagram_2x.png)

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

oddDigits.union(evenDigits).sort()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits.intersect(evenDigits).sort()
// []
oddDigits.subtract(singleDigitPrimeNumbers).sort()
// [1, 9]
oddDigits.exclusiveOr(singleDigitPrimeNumbers).sort()
// [1, 2, 9]
```
## 字典

### 创建一个空字典
```swift
var namesOfIntegers = [Int: String]()
// namesOfIntegers 是一个空的 [Int: String] 字典
```

### 用字典字面量创建字典
```
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

### 访问和修改字典
我们可以通过字典的只读属性count来获取某个字典的数据项数量：
```swift
print("The dictionary of airports contains \(airports.count) items.")
// 打印 "The dictionary of airports contains 2 items."（这个字典有两个数据项）
```
使用布尔属性isEmpty来快捷地检查字典的count属性是否等于0：
```swift
if airports.isEmpty {
    print("The airports dictionary is empty.")
} else {
    print("The airports dictionary is not empty.")
}
// 打印 "The airports dictionary is not empty."
```

我们还可以使用下标语法来通过给某个键的对应值赋值为nil来从字典里移除一个键值对：
```swift
airports["APL"] = "Apple Internation"
// "Apple Internation" 不是真的 APL 机场, 删除它
airports["APL"] = nil
// APL 现在被移除了
```
updateValue(_:forKey:)方法会返回对应值的类型的可选值。
```swift
if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
    print("The old value for DUB was \(oldValue).")
}
// 输出 "The old value for DUB was Dublin."
```
removeValueForKey(_:)方法也可以用来在字典中移除键值对。这个方法在键值对存在的情况下会移除该键值对并且返回被移除的值或者在没有值的情况下返回nil：
```swift
if let removedValue = airports.removeValueForKey("DUB") {
    print("The removed airport's name is \(removedValue).")
} else {
    print("The airports dictionary does not contain a value for DUB.")
}
// prints "The removed airport's name is Dublin Airport."
```

### 字典遍历

```swift
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
// YYZ: Toronto Pearson
// LHR: London Heathrow
```

```swift
for airportCode in airports.keys {
    print("Airport code: \(airportCode)")
}
// Airport code: YYZ
// Airport code: LHR

for airportName in airports.values {
    print("Airport name: \(airportName)")
}
// Airport name: Toronto Pearson
// Airport name: London Heathrow
```

如果我们只是需要使用某个字典的键集合或者值集合来作为某个接受Array实例的 API 的参数，可以直接使用keys或者values属性构造一个新数组：
```swift
let airportCodes = [String](airports.keys)
// airportCodes 是 ["YYZ", "LHR"]

let airportNames = [String](airports.values)
// airportNames 是 ["Toronto Pearson", "London Heathrow"]
```