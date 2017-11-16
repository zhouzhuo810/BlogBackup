---
title: Android疑难杂症-ViewPager+分组列表+指示器悬停
date: 2017-11-15 17:17:04
tags:
	- Android
categories: Android疑难杂症
---

## 系统自带方案

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >
  
    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">
  
        <android.support.design.widget.CollapsingToolbarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:expandedTitleMarginEnd="64dp"
            app:expandedTitleMarginStart="48dp"
            app:layout_scrollFlags="scroll|exitUntilCollapsed">
  
            <ImageView
                android:id="@+id/main.backdrop"
                android:layout_width="wrap_content"
                android:layout_height="300dp"
                android:scaleType="centerCrop"
                android:src="@drawable/material_img"
                app:layout_collapseMode="parallax" />
  
            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?android:attr/actionBarSize"
                app:layout_collapseMode="pin"  />
  
        </android.support.design.widget.CollapsingToolbarLayout>
    </android.support.design.widget.AppBarLayout>
  
    <android.support.v4.widget.NestedScrollView
  
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:paddingTop="50dp"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">
  
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/my_txt"
            android:textSize="20sp" />
  
    </android.support.v4.widget.NestedScrollView>
  
</android.support.design.widget.CoordinatorLayout>

```
此方案不利于屏幕适配，故不采用。

<!-- more -->

## 指示器悬停

参考的张鸿洋大神的Demo。
[张鸿洋大神的Demo](http://blog.csdn.net/lmj623565791/article/details/43649913)

### 新建values/ids_sticky_nav_layout.xml.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <item name="id_stickynavlayout_topview" type="id"/>
    <item name="id_stickynavlayout_viewpager" type="id"/>
    <item name="id_stickynavlayout_indicator" type="id"/>
    <item name="id_stickynavlayout_innerscrollview" type="id"/>
</resources>
```

### 新建StickyNavLayout

```java
package com.example.demo.view.widget.stickynavlayout;
   
import android.animation.ValueAnimator;
import android.content.Context;
import android.support.v4.view.NestedScrollingParent;
import android.support.v4.view.ViewCompat;
import android.support.v4.view.ViewPager;
import android.support.v4.widget.SwipeRefreshLayout;
import android.support.v7.widget.RecyclerView;
import android.util.AttributeSet;
import android.util.Log;
import android.view.VelocityTracker;
import android.view.View;
import android.view.ViewConfiguration;
import android.view.ViewGroup;
import android.view.animation.Interpolator;
import android.widget.LinearLayout;
import android.widget.OverScroller;
   
import com.example.demo.R;
   
public class StickyNavLayout extends LinearLayout implements NestedScrollingParent {
    private static final String TAG = "StickyNavLayout";
  
    @Override
    public boolean onStartNestedScroll(View child, View target, int nestedScrollAxes) {
        Log.e(TAG, "onStartNestedScroll");
        return true;
    }
  
    @Override
    public void onNestedScrollAccepted(View child, View target, int nestedScrollAxes) {
        Log.e(TAG, "onNestedScrollAccepted");
    }
  
    @Override
    public void onStopNestedScroll(View target) {
        Log.e(TAG, "onStopNestedScroll");
    }
  
    @Override
    public void onNestedScroll(View target, int dxConsumed, int dyConsumed, int dxUnconsumed, int dyUnconsumed) {
        Log.e(TAG, "onNestedScroll");
    }
  
    @Override
    public void onNestedPreScroll(View target, int dx, int dy, int[] consumed) {
        Log.e(TAG, "onNestedPreScroll");
        boolean hiddenTop = dy > 0 && getScrollY() < mTopViewHeight;
        boolean showTop = dy < 0 && getScrollY() >= 0 && !ViewCompat.canScrollVertically(target, -1);

        if (hiddenTop || showTop) {
            scrollBy(0, dy);
            consumed[1] = dy;
        }
    }
  
    private int TOP_CHILD_FLING_THRESHOLD = 3;
  
    @Override
    public boolean onNestedFling(View target, float velocityX, float velocityY, boolean consumed) {
        //如果是recyclerView 根据判断第一个元素是哪个位置可以判断是否消耗
        //这里判断如果第一个元素的位置是大于TOP_CHILD_FLING_THRESHOLD的
        //认为已经被消耗，在animateScroll里不会对velocityY<0时做处理
        if ((target instanceof RecyclerView) && velocityY < 0) {
            final RecyclerView recyclerView = (RecyclerView) target;
            final View firstChild = recyclerView.getChildAt(0);
            final int childAdapterPosition = recyclerView.getChildAdapterPosition(firstChild);
            consumed = childAdapterPosition > TOP_CHILD_FLING_THRESHOLD;
        }
        if (!consumed) {
            animateScroll(velocityY, computeDuration(0), consumed);
        } else {
            animateScroll(velocityY, computeDuration(velocityY), consumed);
        }
        return true;
    }
  
    @Override
    public boolean onNestedPreFling(View target, float velocityX, float velocityY) {
        //不做拦截 可以传递给子View
        return false;
    }
  
    @Override
    public int getNestedScrollAxes() {
        Log.e(TAG, "getNestedScrollAxes");
        return 0;
    }
  
    /**
     * 根据速度计算滚动动画持续时间
     *
     * @param velocityY
     * @return
     */
    private int computeDuration(float velocityY) {
        final int distance;
        if (velocityY > 0) {
            distance = Math.abs(mTop.getHeight() - getScrollY());
        } else {
            distance = Math.abs(mTop.getHeight() - (mTop.getHeight() - getScrollY()));
        }
  
        final int duration;
        velocityY = Math.abs(velocityY);
        if (velocityY > 0) {
            duration = 3 * Math.round(1000 * (distance / velocityY));
        } else {
            final float distanceRatio = (float) distance / getHeight();
            duration = (int) ((distanceRatio + 1) * 150);
        }
  
        return duration;

    }
  
    private void animateScroll(float velocityY, final int duration, boolean consumed) {
        final int currentOffset = getScrollY();
        final int topHeight = mTop.getHeight();
        if (mOffsetAnimator == null) {
            mOffsetAnimator = new ValueAnimator();
            mOffsetAnimator.setInterpolator(mInterpolator);
            mOffsetAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
                @Override
                public void onAnimationUpdate(ValueAnimator animation) {
                    if (animation.getAnimatedValue() instanceof Integer) {
                        scrollTo(0, (Integer) animation.getAnimatedValue());
                    }
                }
            });
        } else {
            mOffsetAnimator.cancel();
        }
        mOffsetAnimator.setDuration(Math.min(duration, 600));
  
        if (velocityY >= 0) {
            mOffsetAnimator.setIntValues(currentOffset, topHeight);
            mOffsetAnimator.start();
        } else {
            //如果子View没有消耗down事件 那么就让自身滑倒0位置
            if (!consumed) {
                mOffsetAnimator.setIntValues(currentOffset, 0);
                mOffsetAnimator.start();
            }

        }
    }
  
    private View mTop;
    private View mNav;
    private ViewPager mViewPager;
  
    private int mTopViewHeight;
  
    private OverScroller mScroller;
    private VelocityTracker mVelocityTracker;
    private ValueAnimator mOffsetAnimator;
    private Interpolator mInterpolator;
    private int mTouchSlop;
    private int mMaximumVelocity, mMinimumVelocity;
  
    private float mLastY;
    private boolean mDragging;
  
    public StickyNavLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        setOrientation(LinearLayout.VERTICAL);

        mScroller = new OverScroller(context);
        mTouchSlop = ViewConfiguration.get(context).getScaledTouchSlop();
        mMaximumVelocity = ViewConfiguration.get(context)
                .getScaledMaximumFlingVelocity();
        mMinimumVelocity = ViewConfiguration.get(context)
                .getScaledMinimumFlingVelocity();

    }
  
    private void initVelocityTrackerIfNotExists() {
        if (mVelocityTracker == null) {
            mVelocityTracker = VelocityTracker.obtain();
        }
    }
  
    private void recycleVelocityTracker() {
        if (mVelocityTracker != null) {
            mVelocityTracker.recycle();
            mVelocityTracker = null;
        }
    }
  
//    @Override
//    public boolean onTouchEvent(MotionEvent event)
//    {
//        initVelocityTrackerIfNotExists();
//        mVelocityTracker.addMovement(event);
//        int action = event.getAction();
//        float y = event.getY();
//
//        switch (action)
//        {
//            case MotionEvent.ACTION_DOWN:
//                if (!mScroller.isFinished())
//                    mScroller.abortAnimation();
//                mLastY = y;
//                return true;
//            case MotionEvent.ACTION_MOVE:
//                float dy = y - mLastY;
//
//                if (!mDragging && Math.abs(dy) > mTouchSlop)
//                {
//                    mDragging = true;
//                }
//                if (mDragging)
//                {
//                    scrollBy(0, (int) -dy);
//                }
//
//                mLastY = y;
//                break;
//            case MotionEvent.ACTION_CANCEL:
//                mDragging = false;
//                recycleVelocityTracker();
//                if (!mScroller.isFinished())
//                {
//                    mScroller.abortAnimation();
//                }
//                break;
//            case MotionEvent.ACTION_UP:
//                mDragging = false;
//                mVelocityTracker.computeCurrentVelocity(1000, mMaximumVelocity);
//                int velocityY = (int) mVelocityTracker.getYVelocity();
//                if (Math.abs(velocityY) > mMinimumVelocity)
//                {
//                    fling(-velocityY);
//                }
//                recycleVelocityTracker();
//                break;
//        }
//
//        return super.onTouchEvent(event);
//    }
    
    @Override
    protected void onFinishInflate() {
        super.onFinishInflate();
        mTop = findViewById(R.id.id_stickynavlayout_topview);
        mNav = findViewById(R.id.id_stickynavlayout_indicator);
        View view = findViewById(R.id.id_stickynavlayout_viewpager);
        if (!(view instanceof ViewPager)) {
            throw new RuntimeException(
                    "id_stickynavlayout_viewpager show used by ViewPager !");
        }
        mViewPager = (ViewPager) view;
    }
    
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        //不限制顶部的高度
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        getChildAt(0).measure(widthMeasureSpec, MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED));
        ViewGroup.LayoutParams params = mViewPager.getLayoutParams();
        params.height = getMeasuredHeight() - mNav.getMeasuredHeight();
        setMeasuredDimension(getMeasuredWidth(), mTop.getMeasuredHeight() + mNav.getMeasuredHeight() + mViewPager.getMeasuredHeight());

    }
  
    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        mTopViewHeight = mTop.getMeasuredHeight();
        Log.e("TTT", "height=" + mTopViewHeight);
    }
  
    public void fling(int velocityY) {
        mScroller.fling(0, getScrollY(), 0, velocityY, 0, 0, 0, mTopViewHeight);
        invalidate();
    }
  
    @Override
    public void scrollTo(int x, int y) {
        if (y < 0) {
            y = 0;
        }
        if (y > mTopViewHeight) {
            y = mTopViewHeight;
        }
        if (y != getScrollY()) {
            super.scrollTo(x, y);
        }
    }
  
    @Override
    public void computeScroll() {
        if (mScroller.computeScrollOffset()) {
            scrollTo(0, mScroller.getCurrY());
            invalidate();
        }
    }
  
}

```

### 布局结构

```xml
            
        <com.example.demo.view.widget.stickynavlayout.StickyNavLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">
            
            <!-- top view -->
            <RelativeLayout
                android:id="@id/id_stickynavlayout_topview"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:background="#4400ff00">
                <com.youth.banner.Banner
                    android:id="@+id/banner"
                    android:layout_width="match_parent"
                    android:layout_height="400px" />
            </RelativeLayout>
            
            <!-- 指示器 -->
            <com.example.demo.view.widget.PagerSlidingTabStrip
                android:id="@id/id_stickynavlayout_indicator"
                android:layout_width="match_parent"
                android:layout_height="@dimen/activity_title_height"
                android:background="@color/white"
                zz:pstsDividerColor="@android:color/transparent"
                zz:pstsIndicatorColor="@color/colorMain"
                zz:pstsIndicatorHeight="3px"
                zz:pstsShouldExpand="false"
                zz:pstsUnderlineColor="@android:color/transparent"
                zz:pstsUnderlineHeight="6px"
                zz:zmsSelectedTabTextColor="@color/colorMain"
                zz:zmsSelectedTabTextSize="34px"
                zz:zmsTabTextColor="@android:color/black"
                zz:zmsTabTextSize="34px" />
            
            <!-- ViewPager -->
            <android.support.v4.view.ViewPager
                android:id="@id/id_stickynavlayout_viewpager"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />

        </com.example.demo.view.widget.stickynavlayout.StickyNavLayout>
```


## 分组列表

本来考虑使用ExpandableListView，但是和StickyNavLayout不兼容。

于是找了另一个解决方案：

[BaseRecyclerViewAdapterHelper](https://github.com/CymChad/BaseRecyclerViewAdapterHelper)

### 添加依赖

```
    allprojects {
        repositories {
            ...
            maven { url "https://jitpack.io" }
        }
    }
```

```
    dependencies {
            compile 'com.github.CymChad:BaseRecyclerViewAdapterHelper:2.9.30'
    }
```

### 定义分组适配器

```java
package com.keqiang.highcloud.view.adapter;
  
import android.view.View;
import android.view.ViewGroup;
  
import com.chad.library.adapter.base.BaseMultiItemQuickAdapter;
import com.chad.library.adapter.base.BaseViewHolder;
import com.chad.library.adapter.base.entity.MultiItemEntity;
import com.keqiang.highcloud.R;
import com.keqiang.highcloud.model.http.entity.test.MachineDetailsTabEntity;
import com.keqiang.highcloud.utils.display.ViewUtil;
  
import java.util.List;
  
/**
 * Created by user999 on 2016/6/7.
 */
public class MachineDetailTabAdapter extends BaseMultiItemQuickAdapter<MultiItemEntity, BaseViewHolder> {
  
    public static final int TYPE_GROUP = 1;
    public static final int TYPE_CHILD = 2;
  
    /**
     * Same as QuickAdapter#QuickAdapter(Context,int) but with
     * some initialization data.
     *
     * @param data A new list is created out of this one to avoid mutable list
     */
    public MachineDetailTabAdapter(List<MultiItemEntity> data) {
        super(data);
        addItemType(TYPE_GROUP, R.layout.list_expand_group_item);
        addItemType(TYPE_CHILD, R.layout.list_expand_child_item);
    }
  
    @Override
    protected BaseViewHolder createBaseViewHolder(ViewGroup parent, int layoutResId) {
        BaseViewHolder root = super.createBaseViewHolder(parent, layoutResId);
        ViewUtil.scaleContentView((ViewGroup) root.itemView);
        return root;
    }
  
    @Override
    protected void convert(final BaseViewHolder helper, final MultiItemEntity item) {
        switch (helper.getItemViewType()) {
            case TYPE_GROUP:
                final MachineDetailsTabEntity.DataBean.TasksBean.DetailInfoBean entity = (MachineDetailsTabEntity.DataBean.TasksBean.DetailInfoBean) item;
                switch (entity.getIndex()) {
                    case 0:
                        helper.setImageResource(R.id.list_group_pic, R.drawable.mac_detail_one);
                        helper.setTextColor(R.id.list_group_title, mContext.getResources().getColor(R.color.color_mac_one));
                        break;
                    case 1:
                        helper.setImageResource(R.id.list_group_pic, R.drawable.mac_detail_two);
                        helper.setTextColor(R.id.list_group_title, mContext.getResources().getColor(R.color.color_mac_two));
                        break;
                    case 2:
                        helper.setImageResource(R.id.list_group_pic, R.drawable.mac_detail_three);
                        helper.setTextColor(R.id.list_group_title, mContext.getResources().getColor(R.color.color_mac_three));
                        break;
                    default:
                        helper.setImageResource(R.id.list_group_pic, R.drawable.mac_detail_last);
                        helper.setTextColor(R.id.list_group_title, mContext.getResources().getColor(R.color.color_mac_other));
                        break;
                }
                helper.setText(R.id.list_group_title, entity.getMainTitle());
                helper.itemView.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        int pos = helper.getAdapterPosition();
                        if (entity.isExpanded()) {
                            collapse(pos);
                        } else {
                            expand(pos);
                        }
                    }
                });
                break;
            case TYPE_CHILD:
                final MachineDetailsTabEntity.DataBean.TasksBean.DetailInfoBean.InfoBean entity1 = (MachineDetailsTabEntity.DataBean.TasksBean.DetailInfoBean.InfoBean) item;
                helper.setText(R.id.list_item_machine_detail_tv_device_title, entity1.getTitle());
                helper.setText(R.id.list_item_machine_detail_tv_device_value, entity1.getValue());
                switch (entity1.getIndex()) {
                    case 0:
                        helper.setImageResource(R.id.list_item_machine_detail_iv_device_point, R.drawable.mac_one_shape);
                        break;
                    case 1:
                        helper.setImageResource(R.id.list_item_machine_detail_iv_device_point, R.drawable.mac_two_shape);
                        break;
                    case 2:
                        helper.setImageResource(R.id.list_item_machine_detail_iv_device_point, R.drawable.mac_three_shape);
                        break;
                    default:
                        helper.setImageResource(R.id.list_item_machine_detail_iv_device_point, R.drawable.mac_other_shape);
                        break;
                }
                break;
        }
    }
}
```

### 实体类添加分组功能

- 分组对应实体类必须继承AbstractExpandableItem<组员实体类> 和实现接口MultiItemEntity；
- 组员对应实体类必须实现接口MultiItemEntity。

```java
            public static class DetailInfoBean extends AbstractExpandableItem<DetailInfoBean.InfoBean> implements MultiItemEntity{
  
                private int index = 0;
  
                public int getIndex() {
                    return index;
                }
  
                public void setIndex(int index) {
                    this.index = index;
                }
  
                @Override
                public int getLevel() {
                    return 0;
                }
  
                @Override
                public int getItemType() {
                    return MachineDetailTabAdapter.TYPE_GROUP;
                }
  
                public static class InfoBean implements MultiItemEntity {
  
                    private int index = 0;
  
                    public int getIndex() {
                        return index;
                    }
  
                    public void setIndex(int index) {
                        this.index = index;
                    }
  
	                @Override
	                public int getLevel() {
	                    return 0;
	                }
  
	                @Override
	                public int getItemType() {
	                    return MachineDetailTabAdapter.TYPE_GROUP;
	                }
            }
  
```


### Activity数据填充


```java
    final List<String> tabNames = new ArrayList<>();
  
    for (int i = 0; i < data.getTasks().size(); i++) {
        tabNames.add("任务" + (i + 1));
    }
  
    final List<MachineDetailFragment> fragments = new ArrayList<>();
    for (int i = 0; i < tabNames.size(); i++) {
        fragments.add(MachineDetailFragment.newInstance(macId, tabNames.get(i), data.getTasks().get(i)));
    }
  
    mAdapter = new FragmentPagerAdapter(getSupportFragmentManager()) {
        @Override
        public int getCount() {
            return fragments.size();
        }
  
        @Override
        public Fragment getItem(int position) {
            return fragments.get(position);
        }
  
        @Override
        public CharSequence getPageTitle(int position) {
            return tabNames.get(position);
        }
    };
  
    mViewPager.setAdapter(mAdapter);
    mViewPager.setCurrentItem(0);
    mIndicator.setViewPager(mViewPager);
    mIndicator.updatePosition(0);
```

### Fragment填数据

```java
    List<MultiItemEntity> list = new ArrayList<>();
    List<MachineDetailsTabEntity.DataBean.TasksBean.DetailInfoBean> detailInfo = data.getDetailInfo();
    for (int i = 0; i < detailInfo.size(); i++) {
        MachineDetailsTabEntity.DataBean.TasksBean.DetailInfoBean entity = detailInfo.get(i);
        //设置GroupPosition
        entity.setIndex(i);
        //这个必须加！！！，不然刷新数据会重复添加。
        if (entity.getSubItems() != null) {
            entity.getSubItems().clear();
        }
        if (entity.getInfo() != null) {
            for (MachineDetailsTabEntity.DataBean.TasksBean.DetailInfoBean.InfoBean infoEntity : entity.getInfo()) {
            	//设置GroupPosition
                infoEntity.setIndex(i);
                entity.addSubItem(infoEntity);
            }
        }
        list.add(entity);
    }
    adapter.setNewData(list);
    //为了保证能展示出来，先收起来。
    for (int i = 0; i < detailInfo.size(); i++) {
        //注意！！！，这里如果adapter添加了n个header则i+n。
        adapter.collapse(i+1);
    }
    //全部展开
    adapter.expandAll();
```

### 效果图

![demo](../../../../images/viewpager_rv_banner_demo.gif)

## 潜在解决方案

[Android-ObservableScrollView](https://github.com/zhouzhuo810/Android-ObservableScrollView)

由于时间有限，还没来得及尝试此方案，后续再研究一下。
