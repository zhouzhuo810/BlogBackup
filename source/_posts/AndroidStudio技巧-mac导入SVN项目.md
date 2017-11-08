---
title: AndroidStudio技巧-mac上导入导出SVN项目
date: 2017-11-07 17:20:43
tags:
	- Android
categories: Android开发工具
---


### Android Studio -> SVN

- 1.打开项目
- 2.Preferences -> Version Control -> Ignored Files
（1）app/build
（2）build
（3）*.iml
- 3.VCS -> import into version control -> import into subversion
- 4.Submit


### SVN -> Android Studio

- 1.Check out project from version control
- 2.选择Subversion
- 3.选中项目文件夹根目录
- 4.选择保存路径(非中文路径，新建一个项目文件夹)
- 5.等待完成(如果打开会报错,大多是找不到gradle,别急)。
- 6.复制其他项目的local.properties文件到项目文件夹。
- 7.open a new Android Studio project打开项目


