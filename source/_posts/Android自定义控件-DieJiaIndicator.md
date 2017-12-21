---
title: Android自定义控件-DieJiaIndicator
date: 2017-12-21 10:23:38
tags:
	- Android
categories: Android自定义控件
---

## 固定数量效果图

![图2](../../../../images/DieJiaIndicatorDemo2.gif)

## 动态数量效果图

![图1](../../../../images/DieJiaIndicatorDemo.gif)

<!-- more -->

### 固定数量代码实现

- layout关键代码
```xml
<FrameLayout
    android:id="@+id/fl_top_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="10px">
  
    <FrameLayout
        android:id="@+id/fl_top_tabs_childs"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
  
        <ImageView
            android:id="@+id/iv_error"
            android:layout_width="match_parent"
            android:layout_height="85px"
            android:scaleType="fitXY"
            android:src="@drawable/machinestate_error_selected"
            app:layout_auto_baseheight="width" />
  
        <ImageView
            android:id="@+id/iv_working"
            android:layout_width="match_parent"
            android:layout_height="85px"
            android:scaleType="fitXY"
            android:src="@drawable/machinestate_working_selected"
            app:layout_auto_baseheight="width" />
  
        <ImageView
            android:id="@+id/iv_free"
            android:layout_width="match_parent"
            android:layout_height="85px"
            android:scaleType="fitXY"
            android:src="@drawable/machinestate_free_selected"
            app:layout_auto_baseheight="width" />
  
        <ImageView
            android:id="@+id/iv_disconnect"
            android:layout_width="match_parent"
            android:layout_height="85px"
            android:scaleType="fitXY"
            android:src="@drawable/machinestate_disconnect_selected"
            app:layout_auto_baseheight="width" />
  
        <ImageView
            android:id="@+id/iv_all"
            android:layout_width="match_parent"
            android:layout_height="85px"
            android:scaleType="fitXY"
            android:src="@drawable/machinestate_all_selected"
            app:layout_auto_baseheight="width" />
    </FrameLayout>
  
    <LinearLayout
        android:id="@+id/ll_top_tabs_tv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
  
        <TextView
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_marginLeft="40px"
            android:layout_weight="1"
            android:gravity="center"
            android:paddingBottom="15px"
            android:paddingTop="20px"
            android:text="所有 80"
            android:textColor="@color/colorWhite"
            android:textSize="31px" />
  
        <TextView
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:paddingBottom="15px"
            android:paddingTop="20px"
            android:text="失联 20"
            android:textColor="@color/colorWhite"
            android:textSize="31px" />
  
        <TextView
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:paddingBottom="15px"
            android:paddingTop="20px"
            android:text="空闲 20"
            android:textColor="@color/colorWhite"
            android:textSize="31px" />
  
        <TextView
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:paddingBottom="15px"
            android:paddingTop="20px"
            android:text="工作 20"
            android:textColor="@color/colorWhite"
            android:textSize="31px" />
  
        <TextView
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_marginRight="40px"
            android:layout_weight="1"
            android:gravity="center"
            android:paddingBottom="15px"
            android:paddingTop="20px"
            android:text="故障 20"
            android:textColor="@color/colorWhite"
            android:textSize="31px" />
    </LinearLayout>
</FrameLayout>

```

- java关键代码
```java
    for (int i = 0; i < llTopTabsTv.getChildCount(); i++) {
        View childAt = llTopTabsTv.getChildAt(i);
        final int finalI = i;
        childAt.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                setTopTabSelection(finalI);
            }
        });
    }
  
private void setTopTabSelection(int finalI) {
    switch (finalI) {
        case 0:
            flTopTabsChilds.bringChildToFront(ivError);
            flTopTabsChilds.bringChildToFront(ivWorking);
            flTopTabsChilds.bringChildToFront(ivFree);
            flTopTabsChilds.bringChildToFront(ivDisconnect);
            flTopTabsChilds.bringChildToFront(ivAll);
            break;
        case 1:
            flTopTabsChilds.bringChildToFront(ivError);
            flTopTabsChilds.bringChildToFront(ivWorking);
            flTopTabsChilds.bringChildToFront(ivFree);
            flTopTabsChilds.bringChildToFront(ivAll);
            flTopTabsChilds.bringChildToFront(ivDisconnect);
            break;
        case 2:
            flTopTabsChilds.bringChildToFront(ivError);
            flTopTabsChilds.bringChildToFront(ivWorking);
            flTopTabsChilds.bringChildToFront(ivAll);
            flTopTabsChilds.bringChildToFront(ivDisconnect);
            flTopTabsChilds.bringChildToFront(ivFree);
            break;
        case 3:
            flTopTabsChilds.bringChildToFront(ivError);
            flTopTabsChilds.bringChildToFront(ivAll);
            flTopTabsChilds.bringChildToFront(ivDisconnect);
            flTopTabsChilds.bringChildToFront(ivFree);
            flTopTabsChilds.bringChildToFront(ivWorking);
            break;
        case 4:
            flTopTabsChilds.bringChildToFront(ivAll);
            flTopTabsChilds.bringChildToFront(ivDisconnect);
            flTopTabsChilds.bringChildToFront(ivFree);
            flTopTabsChilds.bringChildToFront(ivWorking);
            flTopTabsChilds.bringChildToFront(ivError);
            break;
    }
}
```


### 动态数量代码实现

## 代码

```java
import android.annotation.TargetApi;
import android.content.Context;
import android.graphics.Color;
import android.os.Build;
import android.os.Parcel;
import android.os.Parcelable;
import android.util.AttributeSet;
import android.util.Log;
import android.util.TypedValue;
import android.view.Gravity;
import android.view.View;
import android.widget.FrameLayout;
import android.widget.HorizontalScrollView;
import android.widget.ImageView;
import android.widget.RelativeLayout;
import android.widget.TextView;
  
import com.zhy.autolayout.utils.AutoUtils;
  
import java.util.ArrayList;
import java.util.List;
  
/**
 * 叠加的Tab
 * Created by admin on 2017/12/20.
 */
public class DieJiaIndicator extends HorizontalScrollView {
  
    private int normalDrawableId;
    private int selectDrawableId;
    private List<String> items;
    private FrameLayout flBg;
    private FrameLayout flTv;
    private int position = 0;
    private List<ImageView> imgs;
  
    private OnItemClickListener onItemClickListener;
  
    public interface OnItemClickListener {
        void onItemClick(int position, boolean changed);
    }
  
    public void setOnItemClickListener(OnItemClickListener onItemClickListener) {
        this.onItemClickListener = onItemClickListener;
    }
  
    public DieJiaIndicator(Context context) {
        super(context);
        init(context, null);
    }

    public DieJiaIndicator(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context, attrs);
    }
  
    public DieJiaIndicator(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs);
    }
  
    @TargetApi(Build.VERSION_CODES.LOLLIPOP)
    public DieJiaIndicator(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        init(context, attrs);
    }
  
    private void init(Context context, AttributeSet attrs) {
        setHorizontalScrollBarEnabled(false);
        imgs = new ArrayList<>();
        RelativeLayout rlRoot = new RelativeLayout(context, attrs);
        flBg = new FrameLayout(context, attrs);
        flTv = new FrameLayout(context, attrs);
        rlRoot.addView(flBg);
        rlRoot.addView(flTv);
        addView(rlRoot);
    }
  
    public void setItems(int normalDrawableId, int selectDrawableId, List<String> items) {
        this.normalDrawableId = normalDrawableId;
        this.selectDrawableId = selectDrawableId;
        this.items = items;
        initChildren();
    }
  
    private void initChildren() {
        if (items != null && items.size() > 0) {
            flBg.removeAllViews();
            flTv.removeAllViews();
            imgs.clear();
            for (String item : items) {
                addTab(item);
            }
            setSelection(0);
            initEvent();
        }
    }
  
    private void initEvent() {
        for (int i = 0; i < flTv.getChildCount(); i++) {
            final TextView childAt = (TextView) flTv.getChildAt(i);
            final int finalI = i;
            childAt.setOnClickListener(new OnClickListener() {
                @Override
                public void onClick(View view) {
                    setSelection(finalI);
                }
            });
        }
    }
  
    private void addTab(String msg) {
        final int position = imgs.size();
        ImageView ivBg = new ImageView(getContext());
        int width = AutoUtils.getPercentWidthSize(206);
        int height = AutoUtils.getPercentHeightSize(78);
        int leftMargin = AutoUtils.getPercentWidthSize(170);
        int leftPadding = AutoUtils.getPercentWidthSize(50);
        int bottomPadding = AutoUtils.getPercentHeightSize(10);
  
        LayoutParams lpImg = new LayoutParams(width, height);
        lpImg.leftMargin = leftMargin * position;
        ivBg.setLayoutParams(lpImg);
        ivBg.setScaleType(ImageView.ScaleType.FIT_XY);
        TextView tvMsg = new TextView(getContext());
        tvMsg.setLayoutParams(lpImg);
        tvMsg.setGravity(Gravity.BOTTOM);
        tvMsg.setTextColor(Color.WHITE);
        tvMsg.setPadding(leftPadding, 0, 0, bottomPadding);
        tvMsg.setText(msg);
        ivBg.setImageResource(normalDrawableId);
        flBg.addView(ivBg);
        flTv.addView(tvMsg);
        imgs.add(ivBg);
    }
  
    public void setSelection(int position) {
        int normalTextSize = AutoUtils.getPercentWidthSize(31);
        int largetTextSize = AutoUtils.getPercentWidthSize(36);
        for (int i = 0; i < flTv.getChildCount(); i++) {
            TextView childAt = (TextView) flTv.getChildAt(i);
            childAt.setTextSize(TypedValue.COMPLEX_UNIT_PX, normalTextSize);
        }
        TextView childAt = (TextView) flTv.getChildAt(position);
        childAt.setTextSize(TypedValue.COMPLEX_UNIT_PX, largetTextSize);
        for (int i1 = 0; i1 < imgs.size(); i1++) {
            ImageView childAt1 = imgs.get(i1);
            childAt1.setImageResource(position == i1 ? selectDrawableId : normalDrawableId);
        }
        for (int j = 0; j < position; j++) {
            flBg.bringChildToFront(imgs.get(j));
        }
        for (int j = imgs.size() - 1; j > position; j--) {
            flBg.bringChildToFront(imgs.get(j));
        }
        flBg.bringChildToFront(imgs.get(position));
        if (onItemClickListener != null) {
            onItemClickListener.onItemClick(position, DieJiaIndicator.this.position != position);
        }
        this.position = position;
    }
  
    @Override
    protected Parcelable onSaveInstanceState() {
        Parcelable superState = super.onSaveInstanceState();
        SaveState saveState = new SaveState(superState);
        saveState.position = this.position;
        return saveState;
    }
  
    @Override
    protected void onRestoreInstanceState(Parcelable state) {
        SaveState savedState = (SaveState) state;
        super.onRestoreInstanceState(savedState.getSuperState());
        this.position = savedState.position;
        setSelection(position);
    }
  
    public int getSelection() {
        return position;
    }
  
    static class SaveState extends BaseSavedState {
        int position;
        public static final Creator<SaveState> CREATOR = new Creator<SaveState>() {
            public SaveState createFromParcel(Parcel source) {
                return new SaveState(source);
            }
  
            public SaveState[] newArray(int size) {
                return new SaveState[size];
            }
        };
  
        public int describeContents() {
            return 0;
        }
  
        public void writeToParcel(Parcel dest, int flags) {
            super.writeToParcel(dest, flags);
            dest.writeInt(this.position);
        }
  
        public SaveState(Parcelable superState) {
            super(superState);
        }
  
        protected SaveState(Parcel in) {
            super(in);
            this.position = in.readInt();
        }
    }
}
```

### 使用方式

```java
	indicator.setItems(R.drawable.detailinfotabtitle, 
	R.drawable.detailinfotabtitlecheckedfull, items);
```

```java
    mViewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
        @Override
        public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
  
        }
  
        @Override
        public void onPageSelected(int position) {
            indicator.setSelection(position);
        }
  
        @Override
        public void onPageScrollStateChanged(int state) {
  
        }
    });
```

```java
	indicator.setOnItemClickListener(new DieJiaIndicator.OnItemClickListener() {
	    @Override
	    public void onItemClick(int position, boolean changed) {
	        if (changed) {
	            mViewPager.setCurrentItem(position);
	        }
	    }
	});
```