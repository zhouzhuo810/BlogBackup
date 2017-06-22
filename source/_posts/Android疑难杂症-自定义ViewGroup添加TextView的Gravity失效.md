---
title: Android疑难杂症-自定义ViewGroup添加TextView的Gravity失效
date: 2017-06-13 14:34:39
tags: 
	- Android
categories: Android疑难杂症
---

解决方法：

在addView()之前

添加如下代码

```java
        mTextView.measure(View.MeasureSpec.makeMeasureSpec(mWidth, View.MeasureSpec.EXACTLY),
                View.MeasureSpec.makeMeasureSpec(mHeight, View.MeasureSpec.EXACTLY));
```