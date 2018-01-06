---
title: Android常用代码-一个TextView显示黑色标题和灰色内容
date: 2018-01-05 13:54:09
tags:
	- Android
categories: Android常用代码
---

### 需求

- 标题黑色，内容灰色；
- 标题后紧跟内容；
- 内容换行后和标题对齐；

### 分析

- 两个TextView不能实现；

### 可行方案

- 方案1：一个TextView，使用SpannableString
- 方案2：一个TextView，使用Html标签控制样式

<!-- more -->

### 方法
```java
private SpannableStringBuilder generateTitleContent(String title, String content) {
    SpannableStringBuilder sb = new SpannableStringBuilder(title + content);
    sb.setSpan(new StyleSpan(Typeface.BOLD), 0, title.length(), 
    	Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
    sb.setSpan(new ForegroundColorSpan(Color.BLACK), 0, title.length(), 
    	Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
    sb.setSpan(new ForegroundColorSpan(Color.parseColor("#5f656e")), title.length(), title.length() + content.length(), 
    	Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
    return sb;
}
```

### 使用
```java
	mTextView.setText(generateTitleContent(title, content));
```