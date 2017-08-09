---
title: Android疑难杂症-EditText抢占焦点使ScrollView自动滑动问题
date: 2017-07-18 08:40:40
tags:
	- Android
categories: Android疑难杂症
---

解决方法：

重写ScrollView的这个方法：

```java
    @Override
    public boolean requestChildRectangleOnScreen(View child, Rect rectangle, boolean immediate) {
        return child instanceof EditText;
    }
```