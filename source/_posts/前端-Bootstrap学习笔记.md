---
title: 前端-Bootstrap学习笔记
date: 2018-01-06 20:38:17
tags:
	- 网页前端
categories: 网页前端
---

## 作者

- Twitter

## 功能

-  一个Web开发框架

## 下载

- [getbootstrap](http://getbootstrap.com/)
- [bootcss](http://www.bootcss.com/)
- [bootcnd](http://www.bootcdn.cn/)

<!-- more -->

## 用法

- 官网下载预编译好的压缩包，解压即可；
- 依赖于JQuery，所以必须先引入JQuery；
- 引入css文件和js文件。

### 最简单模版

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
    <!--[if lt IE 9]>
      <script src="https://cdn.bootcss.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://cdn.bootcss.com/respond.js/1.4.2/respond.min.js"></script>
    <![endif]-->
  </head>
  <body>
    <h1>你好，世界！</h1>

    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
    <script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="js/bootstrap.min.js"></script>
  </body>
</html>
```

### 自定义样式

访问如下网址定制：

[https://v3.bootcss.com/customize/](https://v3.bootcss.com/customize/)

### 常用代码

- 访问如下网址查找

[https://v3.bootcss.com/components/](https://v3.bootcss.com/components/)

## 练习题

### 导航栏练习

- 题目

![导航栏练习1](http://img.mukewang.com/5412aa420001eedc08180368.jpg)

- 实现代码

```html
<nav class="navbar navbar-default">
  <div class="container-fluid">
    <!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="#">某管理系统</a>
    </div>
    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav">
        <li class="active"><a href="#">首页<span class="sr-only">(current)</span></a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">功能<span class="caret"></span></a>
          <ul class="dropdown-menu">
            <li class="dropdown-header">业务功能</li>
            <li><a href="#">信息建立</a></li>
            <li><a href="#">信息查询</a></li>
            <li><a href="#">信息管理</a></li>
            <li role="separator" class="divider"></li>
            <li class="dropdown-header">系统功能</li>
            <li><a href="#">设置</a></li>
          </ul>
        </li>
        <li><a href="#">帮助</a></li>
      </ul>
      <form class="navbar-form navbar-right">
        <div class="form-group">
          <input type="text" class="form-control" placeholder="用户名...">
          <input type="text" class="form-control" placeholder="密码...">
        </div>
        <button type="submit" class="btn btn-default">登录</button>
      </form>
    </div><!-- /.navbar-collapse -->
  </div><!-- /.container-fluid -->
</nav>
```







