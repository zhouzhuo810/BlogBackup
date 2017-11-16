---
title: Androidd开源控件-Glide
date: 2017-11-14 14:44:24
tags:
	- Android
categories: Android开源控件
---

## Glide 4.3.1 (4.x用起来更麻烦了)

### 集成

```
repositories {
  mavenCentral()
  maven { url 'https://maven.google.com' }
}

dependencies {
  compile 'com.github.bumptech.glide:glide:4.3.1'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.3.1'
}
```
<!-- more -->

### 清理缓存


```java
	Glide.get(getActivity()).clearMemory();
	new Thread(new Runnable() {
	    @Override
	    public void run() {
	        Glide.get(getActivity()).clearDiskCache();
	        getActivity().runOnUiThread(new Runnable() {
	            @Override
	            public void run() {
	                ToastUtils.showCustomBgToast("清理成功！");
	            }
	        });
	    }
	}).start();
```

### 圆形图片

```java
	 Glide.with(mContext).load(url)
	 .apply(new RequestOptions()
	 	.circleCrop())
	 .into(ivPhoto);
```

### 默认和错误图片

```java
	 Glide.with(mContext).load(url)
	 .apply(new RequestOptions()
	 	.error(R.drawable.photo_me)
	 	.placeholder(R.drawable.photo_me))
	 .into(ivPhoto);
```

### 加载动画

```java
	 Glide.with(mContext).load(url)
	 .transition(new DrawableTransitionOptions().crossFade(300))
	 .into(ivPhoto);
```


## Glide 3.8.0

```
repositories {
  mavenCentral()
  maven { url 'https://maven.google.com' }
}
  
dependencies {
  compile 'com.github.bumptech.glide:glide:3.8.0'
}
```

### 使用

```java
    Glide.with(this)
    .load(url)
    .placeholder(R.drawable.ic_default)
    .error(R.drawable.ic_default)
    .crossFade()
    .listener(new RequestListener<String, GlideDrawable>() {
                    @Override
                    public boolean onException(Exception e, String model, Target<GlideDrawable> target, boolean isFirstResource) {
                        return false;
                    }

                    @Override
                    public boolean onResourceReady(GlideDrawable resource, String model, Target<GlideDrawable> target, boolean isFromMemoryCache, boolean isFirstResource) {
                        return false;
                    }
                })
    .into(iv);
```