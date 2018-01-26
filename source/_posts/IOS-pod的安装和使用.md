---
title: iOS-pod的安装和使用
date: 2017-06-08 15:49:56
tags: 
	- iOS 
categories: iOS 
---

使用pod可以很方便的集成第三方框架。

## 安装


```
sudo gem install cocoapods
```

输入密码

等待即可。

完成标识如下：

```
xx gems installed
```

<!-- more -->

## 验证安装是否成功


```
pod --help
```

## 切换目录

切换到目标工程文件夹

```
cd xxx
```

```
pod init
```

```
ls -al
```

会发现多了一个Podfile文件

## 编辑Podfile文件

```
vi Podfile
```

点击i进入编辑模式

移动光标，在需要的位置添加内容
下面以SDWebImage为例：

```
platform :ios, '8.0'
pod 'SDWebImage', '~>3.8'
use_frameworks!
```

点击esc

点击shift+；

输入wq，回车

## 安装包

先关闭xcode

```
pod install
```

## 注意

会发现多了一个workspace的工程

如果以后要用集成了POD的工程的话就要用workspace的工程。

打开工程使用这个。