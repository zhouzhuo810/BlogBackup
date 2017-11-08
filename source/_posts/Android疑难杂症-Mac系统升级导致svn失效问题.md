---
title: Android疑难杂症-Mac系统升级导致svn失效问题
date: 2017-11-08 13:20:04
tags:
	- Android
categories: Android疑难杂症
---

## 问题描述

Mac升级系统后，打开Android Studio的老项目，svn没法使用了。

且打开终端，输入svn upgrade会出现如下问题。

```
Agreeing to the Xcode/iOS license requires admin privileges, please re-run as root via sudo.
```

<!-- more -->

## 解决方案

### 升级之后权限问题

1、打开终端，输入  
```
sudo xcodebuild -license
```

2、终端提示敲回车键（enter）打开许可协议，照做

3、终端提示 按下  “space” 键阅读许可协议，按“q” 不阅读

4、最终，终端会出现三个选项，agree 、print、cancel，不用想，能不是agree 吗！输入agree，然后enter

### 还原svn

1、终端，切换到项目根目录

2、输入
```
svn upgrade
```

3、问题解决。