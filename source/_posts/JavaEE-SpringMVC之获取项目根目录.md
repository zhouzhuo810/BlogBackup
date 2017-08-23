---
title: JavaEE-SpringMVC之获取项目根目录
date: 2017-08-23 11:03:03
tags:
	- JavaEE
categories: SpringMVC
---

### 代码很少，但是很有用

```java
HttpServletRequest request = ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes()).getRequest();
String realPath = request.getRealPath("");
```
