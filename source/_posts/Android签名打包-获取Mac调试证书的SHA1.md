---
title: Android签名打包-获取Mac调试证书的SHA1
date: 2017-07-28 08:46:46
tags:
	- Android
categories: Android签名打包
---

### 打开Android Studio

### 进入Terminal

### 输入如下命令：

```
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```