---
title: SQL-快速将Excel的数据导入数据库
date: 2017-07-28 08:55:58
tags:
	- SQL 
categories: SQL 
---


### 关键代码


```
=CONCATENATE("INSERT INTO TableName (Colomn1, Column2) values ('"&A1&"','"&B1&"');")
````

### 效果图

![图1](../../../../images/excel.gif)