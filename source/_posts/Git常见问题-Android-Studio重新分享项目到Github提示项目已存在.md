---
title: Git常见问题-Android Studio重新分享项目到Github提示项目已存在
date: 2017-10-16 15:35:02
tags:
	- Git
categories: Git常见问题
---

- 打开Git Bash
- cd ${项目跟目录}
- git remote rm origin
- git remote add origin git@github.com:${用户名}/${项目名}
- vi .git/config
- 删除[remote "origin"]那一行
(vi操作方法，输入i进入输入模式，删除该行之后，按下Esc，输入":wq"保存并退出)