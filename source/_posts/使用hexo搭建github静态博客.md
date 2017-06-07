---
title: 使用hexo搭建github静态博客
date: 2017-06-01 16:19:18
tags: 
	- hexo
categories: hexo
---

网上虽然不乏此类教程，但是实际操作起来还是遇到了一些问题。
总结如下。

以Windows 7系统为例：

### 登陆Github，创建Reposity

1. 用户名.github.io, 例如我的是zhouzhuo810.github.io
2. 点击Setting->Choose Theme->随便选一个Theme->Select->默认Readme
3. 访问username.github.io可以看到readme的内容即可。

### 安装Node.js
https://nodejs.org/en/
选择第二个下载安装。

<!-- more -->  

### 安装git

### 安装hexo
使用cmd命令行工具，

```
F:
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server 
```
### 配置config

 
```
title: 标题
subtitle: 副标题
description: 描述
author: 作者
language: zh-CN 
timezone: Asia/Shanghai
```

 
```  
deploy:
    type: git 
    repo: 复制github的ssh格式的路径(不要用https格式的)
    branch: master
```


### 配置ssh key。

1. 打开git bash
```
$ git config --global user.name "你的github用户名"
$ git config --global user.email "你的github验证邮箱"
$ ssh-keygen -t rsa -C "你的github验证邮箱"
```

一路回车即可。

默认在**C:\Users\xxx\.ssh**下

找到**id_rsa.pub**文件。

2. 
记事本打开，复制所有内容。

到
https://github.com/settings/keys
配置即可

标题随意。

3. 
最后别忘了最关键的一步：

验证SSH key配置是否成功：

```
$ ssh -T git@github.com
```

如果弹出
Are you sure you want to continue connecting (yes/no)? 
输入yes，回车

等待结果：
如果返回以下内容，说明成功了。
```
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```

### 发布

 
```
hexo g

hexo d
```

### 查看

不出意外的话，通过username.github.io可以访问到发布的内容了。


### 切换主题

https://github.com/stkevintan/hexo-theme-material-flow

1.安装插件和下载主题

```
# change to work dir
cd /your_blog_dir/
# install dependencies
npm i -S hexo-generator-search hexo-generator-feed hexo-renderer-less hexo-autoprefixer hexo-generator-json-content
# download source
git clone https://github.com/stkevintan/hexo-theme-material-flow themes/material-flow
```

注意事项：多个插件同时安装报错时，拆分成单个插件安装即可。

如：

```
npm i -S hexo-generator-search

npm i -S hexo-generator-feed

...

...

```

2.修改配置文件_config.yml

```
avatar: /images/avatar.jpg
favicon: /images/favicon.ico

theme: material-flow
search:
  path: search.xml
  field: post

autoprefixer:
  exclude:
    - '*.min.css'
  # remove: false # prevent autoprefixer remove page-break-inside
  # browsers:
  #   - 'last 2 versions'
  #   - '> 5%'

# Generator json content
jsonContent:
  meta: false
  keywords: false
  pages:
    title: true
    slug: false
    date: false
    updated: false
    comments: false
    path: false
    link: false
    permalink: true
    excerpt: false
    keywords: false
    text: true
    raw: false
    content: false
  posts:
    title: true
    slug: false
    date: false
    updated: false
    comments: false
    path: false
    link: false
    permalink: true
    excerpt: false
    keywords: false
    text: true
    raw: false
    content: false
    categories: false
    tags: false

feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:

```


3.头像和网站图标配置

在sources文件夹下新建images文件夹即可。
