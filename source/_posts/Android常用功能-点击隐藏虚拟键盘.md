---
title: Android常用代码-点击隐藏虚拟键盘
date: 2017-08-10 11:38:24
tags: 
	- Android
categories: Android常用代码
---


代码非常简单：


```java
    view.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            InputMethodManager imm = (InputMethodManager)v.getContext().getSystemService(INPUT_METHOD_SERVICE);
            imm.hideSoftInputFromWindow(v.getWindowToken(), 0);
        }
    });
```