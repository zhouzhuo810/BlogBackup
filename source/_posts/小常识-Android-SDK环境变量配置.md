---
title: 小常识-Android SDK环境变量配置
date: 2018-01-22 09:09:06
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
ANDROID_HOME
  
//sdk绝对路径
E:\Android\SDK
```
					- 找到Path环境变量,追加如下内容
```
;%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools;
```