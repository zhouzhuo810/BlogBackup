---
title: Swift笔记-可选链
date: 2017-06-06 14:40:32
tags: 
	- Swift 
categories: Swift 
---

## ！
使用感叹号(!)可选链实例

```swift
class Person {
    var residence: Residence?
}
  
class Residence {
    var numberOfRooms = 1
}
  
let john = Person()
  
//将导致运行时错误
let roomCount = john.residence!.numberOfRooms
  
//fatal error: unexpectedly found nil while unwrapping an Optional value
```
想使用感叹号（!）强制解析获得这个人residence属性numberOfRooms属性值，将会引发运行时错误，因为这时没有可以供解析的residence值。

<!-- more -->

## ？

使用问号(?)可选链实例

```swift
class Person {
    var residence: Residence?
}
  
class Residence {
    var numberOfRooms = 1
}
  
let john = Person()
  
// 链接可选residence?属性，如果residence存在则取回numberOfRooms的值
if let roomCount = john.residence?.numberOfRooms {
    print("John 的房间号为 \(roomCount)。")
} else {
    print("不能查看房间号")
}
  
//不能查看房间号
```


## 连接多层链接

```swift
class Person {
    var residence: Residence?
}
  
// 定义了一个变量 rooms，它被初始化为一个Room[]类型的空数组
class Residence {
    var rooms = [Room]()
    var numberOfRooms: Int {
        return rooms.count
    }
    subscript(i: Int) -> Room {
        return rooms[i]
    }
    func printNumberOfRooms() {
        print("房间号为 \(numberOfRooms)")
    }
    var address: Address?
}
  
// Room 定义一个name属性和一个设定room名的初始化器
class Room {
    let name: String
    init(name: String) { self.name = name }
}
  
// 模型中的最终类叫做Address
class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?
    func buildingIdentifier() -> String? {
        if (buildingName != nil) {
            return buildingName
        } else if (buildingNumber != nil) {
            return buildingNumber
        } else {
            return nil
        }
    }
}
  
let john = Person()
  
if let johnsStreet = john.residence?.address?.street {
    print("John 的地址为 \(johnsStreet).")
} else {
    print("不能检索地址")
}
  
//不能检索地址
```

## 对返回可选值的函数进行链接

我们还可以通过可选链接来调用返回可空值的方法，并且可以继续对可选值进行链接。

```swift
class Person {
    var residence: Residence?
}
  
// 定义了一个变量 rooms，它被初始化为一个Room[]类型的空数组
class Residence {
    var rooms = [Room]()
    var numberOfRooms: Int {
        return rooms.count
    }
    subscript(i: Int) -> Room {
        return rooms[i]
    }
    func printNumberOfRooms() {
        print("房间号为 \(numberOfRooms)")
    }
    var address: Address?
}
  
// Room 定义一个name属性和一个设定room名的初始化器
class Room {
    let name: String
    init(name: String) { self.name = name }
}
  
// 模型中的最终类叫做Address
class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?
    func buildingIdentifier() -> String? {
        if (buildingName != nil) {
            return buildingName
        } else if (buildingNumber != nil) {
            return buildingNumber
        } else {
            return nil
        }
    }
}
  
let john = Person()
  
if john.residence?.printNumberOfRooms() != nil {
    print("指定了房间号)")
}  else {
    print("未指定房间号")
}
  
//未指定房间号
```

上例中，对Residence的函数printNumberOfRooms进行链接。

