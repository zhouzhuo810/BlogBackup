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

### 栅格系统

- `.container` 类用于固定宽度并支持响应式布局的容器。

```html
<div class="container">
  <div class="row">
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
  </div>
</div>
```

- `.container-fluid` 类用于 100% 宽度，占据全部视口（viewport）的容器。

```html
<div class="container-fluid">
  <div class="row">
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
    <div class="col-md-1">.col-md-1</div>
  </div>
</div>
```

详见[https://v3.bootcss.com/css/#grid-options](https://v3.bootcss.com/css/#grid-options)

### 自定义样式

访问如下网址定制：

[https://v3.bootcss.com/customize/](https://v3.bootcss.com/customize/)

### 常用样式

- 访问如下网址查找

[https://v3.bootcss.com/css/](https://v3.bootcss.com/css/)

### 常用组件

- 访问如下网址查找

[https://v3.bootcss.com/components/](https://v3.bootcss.com/components/)

### 常用插件

- 访问如下网址查找

[https://v3.bootcss.com/javascript/](https://v3.bootcss.com/javascript/)

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

### 轮播图练习

- 题目

![轮播图练习1](http://img.mukewang.com/5412ac5a0001086009850612.jpg)

- 实现代码

```html
<div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
  <!-- Indicators -->
  <ol class="carousel-indicators">
    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
    <li data-target="#carousel-example-generic" data-slide-to="1"></li>
    <li data-target="#carousel-example-generic" data-slide-to="2"></li>
  </ol>
  
  <!-- Wrapper for slides -->
  <div class="carousel-inner" role="listbox">
    <div class="item active">
      <img src="http://img.mukewang.com/5412ad400001e52014280484.jpg" alt="...">
      <div class="carousel-caption">
        <p>11 英寸 MacBook Air 充电一次可运行长达 9 小时，而 13 英寸机型则可运行长达 12 小时。</p>
      </div>
    </div>
    <div class="item">
      <img src="http://img.mukewang.com/5412ad7c0001d2eb10880541.jpg" alt="...">
      <div class="carousel-caption">
        <p>无论是什么任务，配备 Intel HD Graphics 5000 图形处理器的第四代 Intel Core 处理器都能应对自如。</p>
      </div>
    </div>
    <div class="item">
      <img src="http://img.mukewang.com/5412ae5c0001653b12800644.jpg" alt="...">
      <div class="carousel-caption">
        <p>有了新一代 802.11ac 技术，MacBook Air 令 Wi-Fi 速度超越极限。</p>
      </div>
    </div>
  </div>
  <!-- Controls -->
  <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
  <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
  <span class="sr-only">Previous</span>
  </a>
  <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
  <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
  <span class="sr-only">Next</span>
  </a>
</div>
```

### 栅格系统练习

- 题目

**桌面显示器：**

![](http://img.mukewang.com/5412b0f90001e80e12800575.jpg)

**平板电脑或手机：**

![](http://img.mukewang.com/5412b1140001f03809010640.jpg)

```
任务
1. 搭建基础的bootstrap页面

2. 根据课程所学或官方文档搭建基础的栅格结构

3. 注意

使用 .col-md-offset-* 类可以将列向右侧偏移。这些类实际是通过使用 * 选择器为当前元素增加了左侧的边距（margin）。例如，.col-md-offset-4 类将 .col-md-4 元素向右侧偏移了4个列（column）的宽度。
使用什么样的类前缀，此例子应该使用 .col-md-*
给p添加样式，加边框和padding
4. 文字：慕课网是一家从事互联网免费教学的网络教育公司。秉承"开拓、创新、公平、分享"的精神，将互联网特性全面的应用在教育领域，致力于为教育机构及求学者打造一站式互动在线教育品牌。
```



- 代码实现

```HTML
<!DOCTYPE html>
<html lang="zh-cn">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Hello World</title>
<link rel="stylesheet" href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css">
<style>
p {
  border: solid 1px #eeeeee;
  padding: 5px;
}
</style>
</head>
  
<body>
<div class="container">
  <div class="row">
    <div class="col-md-4">
      <p>慕课网是一家从事互联网免费教学的网络教育公司。秉承"开拓、创新、公平、分享"的精神，将互联网特性全面的应用在教育领域，致力于为教育机构及求学者打造一站式互动在线教育品牌。</p>
    </div>
    <div class="col-md-4">
      <p>慕课网是一家从事互联网免费教学的网络教育公司。秉承"开拓、创新、公平、分享"的精神，将互联网特性全面的应用在教育领域，致力于为教育机构及求学者打造一站式互动在线教育品牌。</p>
    </div>
    <div class="col-md-4">
      <p>慕课网是一家从事互联网免费教学的网络教育公司。秉承"开拓、创新、公平、分享"的精神，将互联网特性全面的应用在教育领域，致力于为教育机构及求学者打造一站式互动在线教育品牌。</p>
    </div>
  </div>
  <div class="row">
    <div class="col-md-4">
      <p>慕课网是一家从事互联网免费教学的网络教育公司。秉承"开拓、创新、公平、分享"的精神，将互联网特性全面的应用在教育领域，致力于为教育机构及求学者打造一站式互动在线教育品牌。</p>
    </div>
    <div class="col-md-4  col-md-offset-4">
      <p>慕课网是一家从事互联网免费教学的网络教育公司。秉承"开拓、创新、公平、分享"的精神，将互联网特性全面的应用在教育领域，致力于为教育机构及求学者打造一站式互动在线教育品牌。</p>
    </div>
  </div>
  <div class="row">
    <div class="col-md-3 col-md-offset-3">
      <p>慕课网是一家从事互联网免费教学的网络教育公司。秉承"开拓、创新、公平、分享"的精神，将互联网特性全面的应用在教育领域，致力于为教育机构及求学者打造一站式互动在线教育品牌。</p>
    </div>
    <div class="col-md-3  col-md-offset-3">
      <p>慕课网是一家从事互联网免费教学的网络教育公司。秉承"开拓、创新、公平、分享"的精神，将互联网特性全面的应用在教育领域，致力于为教育机构及求学者打造一站式互动在线教育品牌。</p>
    </div>
  </div>
  <div class="row">
    <div class="col-md-6 col-md-offset-3">
      <p>慕课网是一家从事互联网免费教学的网络教育公司。秉承"开拓、创新、公平、分享"的精神，将互联网特性全面的应用在教育领域，致力于为教育机构及求学者打造一站式互动在线教育品牌。</p>
    </div>
  </div>
</div>
<script src="http://cdn.bootcss.com/jquery/1.11.1/jquery.min.js"></script>
<script src="https://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
```

### 标签页练习

- 题目

```
基于Bootstrap实现下图所示效果的页面，一个标签页，页面载入完毕后，默认打开第二个标签：
任务
1. 搭建基础的bootstrap页面
2. 根据课程所学或官方文档搭建基础的标签页结构
3. 注意页面载入完毕后，调用标签页的js api，打开第二个标签。
```



![](http://img.mukewang.com/5412b2d90001276908980347.jpg)

- 代码实现

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Hello World</title>
<link rel="stylesheet" href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css">
</head>
  
<body>
<div>
  
  <!-- Nav tabs -->
  <ul class="nav nav-tabs" role="tablist" id="feature-tab">
    <li role="presentation"><a href="#first" aria-controls="first" role="tab" data-toggle="tab">第一项</a>
    </li>
    <li role="presentation"><a href="#second" aria-controls="second" role="tab" data-toggle="tab">第二项</a>
    </li>
    <li role="presentation"><a href="#third" aria-controls="third" role="tab" data-toggle="tab">第三项</a>
    </li>
    <li role="presentation"><a href="#forth" aria-controls="forth" role="tab" data-toggle="tab">第四项</a>
    </li>
  </ul>
  
  <!-- Tab panes -->
  <div class="tab-content">
    <div role="tabpanel" class="tab-pane active" id="first">
      <p>慕课网是做什么的？</p>
      <p>互联网技能教育，免费的哦</p>
    </div>
    <div role="tabpanel" class="tab-pane" id="second">
      <p>慕课网是做什么的？</p>
      <p>互联网技能教育，免费的哦</p>
    </div>
    <div role="tabpanel" class="tab-pane" id="third">
      <p>慕课网是做什么的？</p>
      <p>互联网技能教育，免费的哦</p>
    </div>
    <div role="tabpanel" class="tab-pane" id="forth">
      <p>慕课网是做什么的？</p>
      <p>互联网技能教育，免费的哦</p>
    </div>
  </div>
</div>
<script src="http://cdn.bootcss.com/jquery/1.11.1/jquery.min.js"></script>
<script src="https://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
<script>
  $(function(){
        $('#feature-tab a[href=#second]').tab('show');
    });
</script>
</body>
</html>
```

### 实战演练

- 题目

```
基于Bootstrap实现下图所示效果的页面，一个管理系统的首页，包含：
1.导航栏（带登录和下拉菜单，黑色背景）；
  
2.左侧导航栏（可参考栅格布局，并添加样式）；
  
3.右侧管理面板：栅格布局，包含标题、按钮、面板、警告框、表格、徽章、进度条等多个组件
```



![](http://img.mukewang.com/543747c10001b14d19200943.jpg)

- 代码实现

```HTML
<!DOCTYPE html>
<html lang="zh-cn">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>某管理系统</title>
<link href="css/bootstrap.min.css" rel="stylesheet">
  
<!--[if lt IE 9]>
<script src="js/html5shiv.js"></script>
<script src="js/respond.min.js"></script>
<![endif]-->
<style>
#left-container {
  background-color: #dddddd;
  height: 900px;
  padding-left: 0px;
  padding-right: 0px;
}
#left-container p {
  margin-left: 0px;
  margin-right: 0px;
  padding: 10px 10px 10px 15px;
}
#right-container {
  background-color: #ffffff;
}
#setting {
  margin-top: 20px;
}
.left-table {
  padding-left: 0px;
}
#btn-box {
  margin-bottom: 20px;
}
.pro-title {
  padding-top: 5px;
  padding-bottom: 5px;
}
.progress {
  margin-top: 10px;
}
.badge {
  float: right;
}
</style>
</head>
  
<body>
<nav class="navbar navbar-inverse">
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
        <li class="active"><a href="#">首页<span class="sr-only">(current)</span></a>
        </li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">功能<span class="caret"></span></a>
          <ul class="dropdown-menu">
            <li class="dropdown-header">业务功能</li>
            <li><a href="#">信息建立</a>
            </li>
            <li><a href="#">信息查询</a>
            </li>
            <li><a href="#">信息管理</a>
            </li>
            <li role="separator" class="divider"></li>
            <li class="dropdown-header">系统功能</li>
            <li><a href="#">设置</a>
            </li>
          </ul>
        </li>
        <li><a href="#">帮助</a>
        </li>
      </ul>
      <form class="navbar-form navbar-right">
        <div class="form-group">
          <input type="text" class="form-control" placeholder="用户名...">
          <input type="text" class="form-control" placeholder="密码...">
        </div>
        <button type="submit" class="btn btn-default">登录</button>
      </form>
    </div>
    <!-- /.navbar-collapse -->
  </div>
  <!-- /.container-fluid -->
</nav>
<div class="container-fluid">
  <div class="row">
    <div class="col-md-2" id="left-container">
      <p class="bg-primary">首页</p>
      <ul class="nav nav-pills nav-stacked">
        <li role="presentation"><a href="#">信息建立</a>
        </li>
        <li role="presentation"><a href="#">信息管理</a>
        </li>
        <li role="presentation"><a href="#">信息查询</a>
        </li>
        <li role="presentation" id="setting"><a href="#">设置</a>
        </li>
        <li role="presentation"><a href="#">帮助</a>
        </li>
      </ul>
    </div>
    <div class="col-md-10" id="right-container">
      <h2>管理控制台</h2>
      <hr/>
      <div id="btn-box">
        <!-- Standard button -->
        <button type="button" class="btn btn-default btn-lg">操作1</button>
        <!-- Provides extra visual weight and identifies the primary action in a set of buttons -->
        <button type="button" class="btn btn-primary btn-lg">操作2</button>
        <!-- Indicates a successful or positive action -->
        <button type="button" class="btn btn-success btn-lg">操作3</button>
        <!-- Contextual button for informational alert messages -->
        <button type="button" class="btn btn-info btn-lg">操作4</button>
        <!-- Indicates caution should be taken with this action -->
        <button type="button" class="btn btn-warning btn-lg">操作5</button>
        <!-- Indicates a dangerous or potentially negative action -->
        <button type="button" class="btn btn-danger btn-lg">操作6</button>
      </div>
      <div class="col-md-6 left-table" >
        <div class="panel panel-primary">
          <div class="panel-heading">
            <h3 class="panel-title">最新提醒</h3>
          </div>
          <div class="panel-body">
            <div class="alert alert-info" role="alert">
              <strong>提示 </strong>您的订单（2014001）已被审核通过！ </div>
            <div class="alert alert-warning" role="alert">
              <strong>提示 </strong>您的订单（2014002）已被打回！ </div>
            <div class="alert alert-danger" role="alert">
              <strong>提示 </strong>您的订单（2013001）客户付款延时！ </div>
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <div class="panel panel-primary">
          <div class="panel-heading">
            <h3 class="panel-title">我的任务</h3>
          </div>
          <div class="panel-body">
            <div class="alert alert-info" role="alert"> 订单审批<span class="badge">42</span>
            </div>
            <div class="alert alert-info" role="alert"> 收款确认<span class="badge">20</span>
            </div>
            <div class="alert alert-info" role="alert"> 付款确认<span class="badge">10</span>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-6 left-table" >
        <div class="panel panel-primary">
          <div class="panel-heading">
            <h3 class="panel-title">最新订单</h3>
          </div>
          <div class="panel-body">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th>#</th>
                  <th>产品</th>
                  <th>数量</th>
                  <th>金额</th>
                  <th>业务员</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>1</td>
                  <td>Apple Macbook air</td>
                  <td>10</td>
                  <td>80000</td>
                  <td>王小贱</td>
                </tr>
                <tr>
                  <td>2</td>
                  <td>Apple iPad air</td>
                  <td>20</td>
                  <td>65000</td>
                  <td>尹开花</td>
                </tr>
                <tr>
                  <td>3</td>
                  <td>Apple Macbook pro</td>
                  <td>5</td>
                  <td>50000</td>
                  <td>刘老根</td>
                </tr>
              </tbody>
            </table>
            <!-- Provides extra visual weight and identifies the primary action in a set of buttons -->
            <button type="button" class="btn btn-primary">查看详情>></button>
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <div class="panel panel-primary">
          <div class="panel-heading">
            <h3 class="panel-title">工程进度</h3>
          </div>
          <div class="panel-body">
            <span class="label label-primary pro-title">水井挖掘工程</span>
            <div class="progress">
              <div class="progress-bar" role="progressbar" aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" style="width: 60%;">
                <span class="sr-only">60% Complete</span>
              </div>
            </div>
            <span class="label label-danger pro-title">基建工程</span>
            <div class="progress">
              <div class="progress-bar progress-bar-danger" role="progressbar" aria-valuenow="80" aria-valuemin="0" aria-valuemax="100" style="width: 80%">
                <span class="sr-only">80% Complete (danger)</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
<script src="http://cdn.bootcss.com/jquery/1.11.1/jquery.min.js"></script>
<script src="https://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
```
