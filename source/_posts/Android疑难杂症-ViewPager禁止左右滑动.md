---
title: Android疑难杂症-ViewPager禁止左右滑动
date: 2017-10-31 13:03:00
tags:
	- Android
categories: Android疑难杂症
---


```java
import android.content.Context;
import android.support.v4.view.ViewPager;
import android.util.AttributeSet;
import android.view.MotionEvent;
  
/**
 * Created by zhouzhuo810 on 2017/10/31.
 */
public class NoScrollViewPager extends ViewPager {
    public NoScrollViewPager(Context context) {
        super(context);
    }
  
    public NoScrollViewPager(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
  
    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        return false;
    }
  
    @Override
    public boolean onTouchEvent(MotionEvent ev) {
        return true;
    }
}
```