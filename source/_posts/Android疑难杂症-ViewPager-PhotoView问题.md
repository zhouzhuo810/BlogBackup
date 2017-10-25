---
title: Android疑难杂症-ViewPager+PhotoView问题
date: 2017-10-11 11:39:56
tags:
	- Android
categories: Android疑难杂症
---


## 版本说明

PhotoView版本：

```
//photoView
compile 'com.mcxiaoke.photoview:library:1.2.3'
```

## 问题描述

- ViewPager+PhotoView缩放操作时报错
- ViewPager+PhotoView左右滑动图片加载错位
- 指定显示第几张图片

<!-- more -->

## 解决方式

### 重写ViewPager的onInterceptTouchEvent方法

```java
@Override
public boolean onInterceptTouchEvent(MotionEvent ev) {
	try {
		return super.onInterceptTouchEvent(ev);
	} catch (IllegalArgumentException e) {
		e.printStackTrace();
		return false;
	}
}
```

### 定义Adapter

```java
public static class MultiImagePageAdapter extends PagerAdapter {
  
    private PhotoViewAttacher.OnViewTapListener onViewTapListener;
  
    //设置点击任意一张图片结束预览
    public void setOnViewTapListener(PhotoViewAttacher.OnViewTapListener onViewTapListener) {
        this.onViewTapListener = onViewTapListener;
    }
  
    private final DisplayImageOptions options;
    private List<String> imgs;
  
    public MultiImagePageAdapter(List<String> imgs) {
        options = new DisplayImageOptions.Builder()
                .showImageOnLoading(R.drawable.product_default)
                .showImageForEmptyUri(R.drawable.product_default)
                .showImageOnFail(R.drawable.product_default)
                .bitmapConfig(Bitmap.Config.ARGB_8888)
                .imageScaleType(ImageScaleType.EXACTLY)
                .cacheInMemory(true)
                .cacheOnDisk(true)
                .build();
        this.imgs = imgs;
    }
  
    @Override
    public int getCount() {
        return imgs == null ? 0 : imgs.size();
    }
  
    @Override
    public boolean isViewFromObject(View view, Object object) {
        return view == object;
    }
  
    @Override
    public Object instantiateItem(ViewGroup container, int position) {
        PhotoView photoView = new PhotoView(container.getContext());
        //关键
        container.addView(photoView, ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
        photoView.setScaleType(ImageView.ScaleType.FIT_CENTER);
        photoView.setOnViewTapListener(onViewTapListener);
        //网络加载
        HCApplication.getImageLoader().displayImage(imgs.get(position), photoView, options);
        return photoView;
    }
  
    @Override
    public void destroyItem(ViewGroup container, int position, Object object) {
    	//别忘了
        container.removeView((View) object);
    }
}

```

### 布局

```xml
<com.keqiang.highcloud.view.widget.HackyViewPager
    android:id="@+id/view_pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
  
</com.keqiang.highcloud.view.widget.HackyViewPager>
```

### 初始化


```java
    List<String> imgs = getIntent().getStringArrayListExtra("imgs");
    int position = getIntent().getIntExtra("position", 0);
    if (imgs != null && imgs.size() > 0) {
        MultiImagePageAdapter adapter = new MultiImagePageAdapter(imgs);
        adapter.setOnViewTapListener(new PhotoViewAttacher.OnViewTapListener() {
            @Override
            public void onViewTap(View view, float x, float y) {
                if (Build.VERSION.SDK_INT >= 21) {
                    onBackPressed();
                } else {
                    closeAct();
                }
            }
        });
        viewPager.setAdapter(adapter);
        viewPager.setCurrentItem(position);
    }
```

### 调用

```java
Intent intent = new Intent(activity, MultiImagePreviewActivity.class);
//传入图片和位置信息
intent.putExtra("imgs", imgs);
intent.putExtra("position", position);
if (Build.VERSION.SDK_INT >= 21) {
    ActivityOptionsCompat options = ActivityOptionsCompat.makeSceneTransitionAnimation(activity,
            banner, getString(R.string.transition_product_img));
    ActivityCompat.startActivity(activity, intent, options.toBundle());
} else {
    startActWithIntent(intent);
}
```

### Activity实例

- 布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/root"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/colorBlack"
    android:orientation="vertical">
  
    <com.keqiang.highcloud.view.widget.HackyViewPager
        android:id="@+id/view_pager"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
  
    </com.keqiang.highcloud.view.widget.HackyViewPager>
</LinearLayout>
```

- java


```java
import android.graphics.Bitmap;
import android.os.Build;
import android.os.Bundle;
import android.support.v4.view.PagerAdapter;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
  
import com.nostra13.universalimageloader.core.DisplayImageOptions;
import com.nostra13.universalimageloader.core.assist.ImageScaleType;
  
import java.util.List;
  
import uk.co.senab.photoview.PhotoView;
import uk.co.senab.photoview.PhotoViewAttacher;
  
/**
 * Created by zhouzhuo810 on 2017/10/11.
 */
public class MultiImagePreviewActivity extends BaseActivity {
  
    private HackyViewPager viewPager;
  
    @Override
    public int getLayoutId() {
        return R.layout.activity_multi_img_preview;
    }
  
    @Override
    public void initView() {
        viewPager = (HackyViewPager) findViewById(R.id.view_pager);
    }
  
    @Override
    public void initData() {
  
        List<String> imgs = getIntent().getStringArrayListExtra("imgs");
        int position = getIntent().getIntExtra("position", 0);
        if (imgs != null && imgs.size() > 0) {
            MultiImagePageAdapter adapter = new MultiImagePageAdapter(imgs);
            adapter.setOnViewTapListener(new PhotoViewAttacher.OnViewTapListener() {
                @Override
                public void onViewTap(View view, float x, float y) {
                    if (Build.VERSION.SDK_INT >= 21) {
                        onBackPressed();
                    } else {
                        closeAct();
                    }
                }
            });
            viewPager.setAdapter(adapter);
            viewPager.setCurrentItem(position);
        }
    }
  
    public static class MultiImagePageAdapter extends PagerAdapter {
  
        private PhotoViewAttacher.OnViewTapListener onViewTapListener;
  
        public void setOnViewTapListener(PhotoViewAttacher.OnViewTapListener onViewTapListener) {
            this.onViewTapListener = onViewTapListener;
        }
  
        private final DisplayImageOptions options;
        private List<String> imgs;
  
        public MultiImagePageAdapter(List<String> imgs) {
            options = new DisplayImageOptions.Builder()
                    .showImageOnLoading(R.drawable.product_default)
                    .showImageForEmptyUri(R.drawable.product_default)
                    .showImageOnFail(R.drawable.product_default)
                    .bitmapConfig(Bitmap.Config.ARGB_8888)
                    .imageScaleType(ImageScaleType.EXACTLY)
                    .cacheInMemory(true)
                    .cacheOnDisk(true)
                    .build();
            this.imgs = imgs;
        }
  
        @Override
        public int getCount() {
            return imgs == null ? 0 : imgs.size();
        }
  
        @Override
        public boolean isViewFromObject(View view, Object object) {
            return view == object;
        }
  
        @Override
        public Object instantiateItem(ViewGroup container, int position) {
            PhotoView photoView = new PhotoView(container.getContext());
            container.addView(photoView, ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
            photoView.setScaleType(ImageView.ScaleType.FIT_CENTER);
            photoView.setOnViewTapListener(onViewTapListener);
            HCApplication.getImageLoader().displayImage(imgs.get(position), photoView, options);
            return photoView;
        }
  
        @Override
        public void destroyItem(ViewGroup container, int position, Object object) {
            container.removeView((View) object);
        }
    }
  
    @Override
    public void initEvent() {
    }
  
    @Override
    public void saveState(Bundle bundle) {
    }
  
    @Override
    public void restoreState(Bundle bundle) {
    }
 
    @Override
    public boolean isDefaultBackClose() {
        return true;
    }
}
```