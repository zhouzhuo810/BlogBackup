---
title: JavaEE-Tomcat使用服务开机自启动
date: 2017-10-04 21:40:07
tags:
	- JavaEE
categories: Tomcat
---


## 前提条件

- JDK和Tomcat同为32位或同为64位。
- 配置JDK环境变量。
- 配置Tomcat环境变量。

<!-- more -->

## 设置服务

- 打开cmd
- 切换到tomcat bin目录
- 运行下列命令

```
service.bat install Tomcat
```

## 卸载服务

- 打开cmd
- 切换到tomcat bin目录
- 运行下列命令

```
service.bat uninstall Tomcat
```

## 设置服务开机自启动

- 打开服务管理
- 设置为自动，应用。


