---
title: Android签名打包-获取Windows和Mac系统中调试证书和发布证书的SHA1
date: 2017-07-28 08:46:46
tags:
	- Android
categories: Android签名打包
---

----------------------------------------

## 发布证书

----------------------------------------

## Windows和Mac一样

- 打开Android Studio

- 找到Terminal,studio下方

<!-- more -->

- 键入命令

```
keytool -v -list -keystore keystorePath(例:e:\test.keystore)
```

- 输入密钥库口令，即可查看相关信息

*注意*：如果命令不加-v 是没有MD5信息的


----------------------------------------

## 调试证书

----------------------------------------
## Windows

- win+R

- cmd,回车

- 输入如下命令 (admin是电脑用户名)：

```
keytool -v -list -keystore c:\users\admin\.android\debug.keystore -alias androiddebugkey -storepass android -keypass android
```

## Mac

- 打开Android Studio

- 进入Terminal

- 输入如下命令：

```
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```