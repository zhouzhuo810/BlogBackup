---
title: Android支付宝支付-返回签名错误问题
date: 2017-09-30 16:14:53
tags:
	- Android
categories: Android疑难杂症
---


## 解决方案

### 首先，检查服务端公钥是否为支付宝公钥。（不是App公钥）。

### 其次，检查服务端APP私钥是否和支付宝平台的APP公钥匹配(推荐RSA2)；

### 最后，沙箱环境是否注释。