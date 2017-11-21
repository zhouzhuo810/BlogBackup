---
title: AndroidStudio技巧-Terminal权限问题
date: 2017-11-21 14:07:07
tags:
	- Android
categories: Android开发工具
---

### 系统

macOs Sierra 10.12.6系统

### 工具

Android Studio 2.3.1 Terminal

### 问题描述

执行./gradlew install命令时提示
gradlew: Permission Denied

### 解决方案

```
	chmod +x gradlew
```

再操作即可。