---
title: Android常用代码-切换主题
date: 2017-10-12 14:50:31
tags:
	- Android
categories: Android常用代码
---


### 核心代码

```java
    private void setThemeColor(int colorPrimary, int colorPrimaryDark) {
        mToolbar.setBackgroundResource(colorPrimary);
        mToolbar.setTitleTextColor(ContextCompat.getColor(this, android.R.color.white));
        mToolbar.setNavigationIcon(R.drawable.ic_arrow_back_white_24dp);
        if (Build.VERSION.SDK_INT >= 21) {
            getWindow().setStatusBarColor(ContextCompat.getColor(this, colorPrimaryDark));
        }
        if (Build.VERSION.SDK_INT >= 23) {
            Window window = getWindow();
            int systemUiVisibility = window.getDecorView().getSystemUiVisibility();
            systemUiVisibility &= ~View.SYSTEM_UI_FLAG_LIGHT_STATUS_BAR;
            window.getDecorView().setSystemUiVisibility(systemUiVisibility);
        }
    }
```


### 使用方式

```java
	setThemeColor(R.color.colorPrimary, R.color.colorPrimaryDark);
```