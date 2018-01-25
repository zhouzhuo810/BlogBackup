---
title: 小常识-JDK环境变量配置
date: 2018-01-22 09:09:47
tags:
	- 常识
categories: 常识
---

### 操作步骤

- 我的电脑
	- 属性
		- 高级系统设置
			- 环境变量
				- 系统环境变量
					- 新建
```
JAVA_HOME
  
//jdk绝对路径
C:\Program Files\Java\jdk1.8.0_60
```
					- 新建
```
CLASSPATH
  
.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar;
```
					- 找到Path环境变量,追加如下内容
```
;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
```
