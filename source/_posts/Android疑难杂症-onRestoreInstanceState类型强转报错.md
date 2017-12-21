---
title: Android疑难杂症-onRestoreInstanceState类型强转报错
date: 2017-12-21 10:06:56
tags:
	- Android
categories: Android疑难杂症
---

## 问题重现

### 层级关系
- ViewPager+Indicator
   ->Fragment
   	  ->ViewPager+Indicator
        ->Fragment

<!-- more -->

### 错误信息

```
java.lang.ClassCastException: android.view.AbsSavedState$1 cannot be cast to com.keqiang.highcloud.ui.widget.DieJiaIndicator$SaveState
     at android.view.View.dispatchRestoreInstanceState(View.java:12799)
     at android.view.ViewGroup.dispatchRestoreInstanceState(ViewGroup.java:2637)
     at android.view.ViewGroup.dispatchRestoreInstanceState(ViewGroup.java:2643)
     at android.view.ViewGroup.dispatchRestoreInstanceState(ViewGroup.java:2643)
     at android.view.ViewGroup.dispatchRestoreInstanceState(ViewGroup.java:2643)
     at android.view.View.restoreHierarchyState(View.java:12777)
     at android.support.v4.app.Fragment.restoreViewState(Fragment.java:475)
     at android.support.v4.app.FragmentManagerImpl.moveToState(FragmentManager.java:1329)
     at android.support.v4.app.FragmentManagerImpl.moveFragmentToExpectedState(FragmentManager.java:1528)
     at android.support.v4.app.BackStackRecord.executeOps(BackStackRecord.java:753)
     at android.support.v4.app.FragmentManagerImpl.executeOps(FragmentManager.java:2363)
     at android.support.v4.app.FragmentManagerImpl.executeOpsTogether(FragmentManager.java:2149)
     at android.support.v4.app.FragmentManagerImpl.optimizeAndExecuteOps(FragmentManager.java:2103)
     at android.support.v4.app.FragmentManagerImpl.execSingleAction(FragmentManager.java:1984)
     at android.support.v4.app.BackStackRecord.commitNowAllowingStateLoss(BackStackRecord.java:626)
     at android.support.v4.app.FragmentPagerAdapter.finishUpdate(FragmentPagerAdapter.java:143)
     at android.support.v4.view.ViewPager.populate(ViewPager.java:1268)
     at android.support.v4.view.ViewPager.setCurrentItemInternal(ViewPager.java:668)
     at android.support.v4.view.ViewPager.setCurrentItemInternal(ViewPager.java:630)
     at android.support.v4.view.ViewPager.setCurrentItem(ViewPager.java:622)
     at zhouzhuo810.me.zzandframe.ui.widget.zzpagerindicator.ZzPagerIndicator.setCurrentItem(ZzPagerIndicator.java:248)
     at zhouzhuo810.me.zzandframe.ui.widget.zzpagerindicator.ZzPagerIndicator$3.onClick(ZzPagerIndicator.java:360)
     at android.view.View.performClick(View.java:4438)
     at android.view.View$PerformClick.run(View.java:18422)
     at android.os.Handler.handleCallback(Handler.java:733)
     at android.os.Handler.dispatchMessage(Handler.java:95)
     at android.os.Looper.loop(Looper.java:136)
     at android.app.ActivityThread.main(ActivityThread.java:5045)
     at java.lang.reflect.Method.invokeNative(Native Method)
     at java.lang.reflect.Method.invoke(Method.java:515)
     at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:779)
     at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:595)
     at dalvik.system.NativeStart.main(Native Method)
```

## 问题分析

- 上层ViewPager重新创建Fragment时下层ViewPager中的Fragment恢复状态时类型强转报错。


## 解决方案

- 方案1：避免重新创建

```java 
    //如果页数固定，直接设置页数；如果动态，尽可能设置到大于页数。
	viewPager.setOffscreenPageLimit(一个较大值);
```

- 方案2：寻找强转类型报错的原因(暂未找到，有人说是Id重复使用问题)