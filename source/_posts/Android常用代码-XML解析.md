---
title: Android常用代码-XML解析
date: 2017-10-18 17:06:52
tags:
	- Android
categories: Android常用代码
---


## 常见XML解析方法

- SAX
- DOM
- PULL
- Dom4j

## 推荐Dom4j

### 下载地址

- jar百度云下载地址

[https://pan.baidu.com/s/1i3hduPf](https://pan.baidu.com/s/1i3hduPf)


### 用法简介

3个概念：元素，属性，值。

<!-- more -->

#### 初始化

```java
    SAXReader saxReader = new SAXReader();
    Document document = saxReader.read(file);
    //获取根节点
    Element root = document.getRootElement();
```

#### 元素

- 获取所有子元素

```java
	List<Element> elements = root.elements();
```

- 获取某一个名为username子元素

```java
	Element element = root.element("username");
```

#### 属性

- 获取元素的某个属性的值

```java
	String firstName = root.attributeValue("firstName");
```

#### 值

```java
//获取username元素的值
	String username = root.elementText("username");
```

也可以这么写：

```java
	String username = root.element("username").getText();
```

### 总结

Dom4j性能非常好，灵活性高，易使用.