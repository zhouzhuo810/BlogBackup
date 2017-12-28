---
title: Android自定义控件-RatioColorBar
date: 2017-12-28 13:17:35
tags:
	- Android
categories: Android自定义控件
---

## 效果图

![图2](../../../../images/RatioColorBar.png)

## 代码

### 属性
```xml
<declare-styleable name="RatioColorBar">
    <attr name="rcb_show_border" format="boolean" />
    <attr name="rcb_show_padding" format="boolean" />
    <attr name="rcb_border_width" format="dimension|reference" />
    <attr name="rcb_border_padding" format="dimension|reference" />
    <attr name="rcb_border_color" format="color|reference" />
</declare-styleable>
```

<!-- more -->

### java代码
```java
import android.annotation.TargetApi;
import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.os.Build;
import android.support.annotation.Nullable;
import android.util.AttributeSet;
import android.util.Log;
import android.view.View;
  
import com.zhy.autolayout.utils.AutoUtils;
  
import java.util.List;
  
/**
 * 彩虹条
 * Created by zhouzhuo810 on 2017/12/28.
 */
public class RatioColorBar extends View {
  
    private boolean showBorder;
    private boolean showPadding;
    private int borderWidth;
    private int borderColor;
    private int padding;
    private List<RatioBarData> colorBars;
  
    private Paint borderPaint;
    private Paint barPaint;
  
    public RatioColorBar(Context context) {
        super(context);
        init(context, null);
    }
  
    public RatioColorBar(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        init(context, attrs);
    }
  
    public RatioColorBar(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs);
    }
  
    @TargetApi(Build.VERSION_CODES.LOLLIPOP)
    public RatioColorBar(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        init(context, attrs);
    }
  
    private void init(Context context, AttributeSet attrs) {
        if (attrs != null) {
            TypedArray t = context.obtainStyledAttributes(attrs, R.styleable.RatioColorBar);
            showBorder = t.getBoolean(R.styleable.RatioColorBar_rcb_show_border, false);
            showPadding = t.getBoolean(R.styleable.RatioColorBar_rcb_show_padding, false);
            borderColor = t.getColor(R.styleable.RatioColorBar_rcb_border_color, 0xffeeeeee);
            borderWidth = t.getDimensionPixelSize(R.styleable.RatioColorBar_rcb_border_width, 1);
            padding = t.getDimensionPixelSize(R.styleable.RatioColorBar_rcb_border_padding, 1);
            t.recycle();
        } else {
            showBorder = false;
            showPadding = false;
            borderColor = 0xffeeeeee;
            borderWidth = 1;
            padding = 1;
        }
        borderWidth = AutoUtils.getPercentWidthSize(borderWidth);
        padding = AutoUtils.getPercentWidthSize(padding);
  
        initPaints();
    }
  
    private void initPaints() {
        borderPaint = new Paint();
        borderPaint.setAntiAlias(true);
        borderPaint.setColor(borderColor);
        borderPaint.setStyle(Paint.Style.STROKE);
        borderPaint.setStrokeWidth(borderWidth);
  
        barPaint = new Paint();
        barPaint.setAntiAlias(true);
        barPaint.setStyle(Paint.Style.FILL);
    }
  
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        if (colorBars != null) {
            int startX = 0;
            int endX = getWidth();
            int startY = 0;
            int endY = getHeight();
            if (showBorder) {
                startX += borderWidth;
                endX -= borderWidth;
                startY += borderWidth;
                endY -= borderWidth;
                canvas.drawRect(startX, startY, endX, endY, borderPaint);
            }
            if (showPadding) {
                startX += padding;
                endX -= padding;
                startY += padding;
                endY -= padding;
            }
            Log.e("RRR", "sx=" + startX + ",ex=" + endX + ",sy=" + startY + ",ey=" + endY);
            int dx = endX - startX;
            Log.e("RRR", "dx=" + dx);
            long sum = 0;
            for (RatioBarData colorBar : colorBars) {
                sum += colorBar.getValue();
            }
            Log.e("RRR", "sum=" + sum);
            if (sum != 0) {
                int realStartX = startX;
                for (RatioBarData colorBar : colorBars) {
                    int color = colorBar.getColor();
                    int ratioWidth = (int) ((dx * (colorBar.getValue() * 1.0 / sum)) + 0.5f);
                    Log.e("RRR", "ratioWidth=" + ratioWidth);
                    barPaint.setColor(color);
                    Log.e("RRR", "realStartX=" + realStartX);
                    canvas.drawRect(realStartX, startY, realStartX + ratioWidth, endY, barPaint);
                    realStartX += ratioWidth;
                }
            }
        }
    }
  
    public void setColorBars(List<RatioBarData> colorBars) {
        this.colorBars = colorBars;
        invalidate();
    }
  
    public static class RatioBarData {
        private long value;
        private int color;
  
        public RatioBarData() {
        }
  
        public RatioBarData(long value, int color) {
            this.value = value;
            this.color = color;
        }
  
        public long getValue() {
            return value;
        }
  
        public void setValue(long value) {
            this.value = value;
        }
  
        public int getColor() {
            return color;
        }
  
        public void setColor(int color) {
            this.color = color;
        }
    }
}
```

### 用法

```xml
<!--注意包名改成自己的-->
<com.example.slidingverificationtest.ui.widget.RatioColorBar
    android:id="@+id/rcb"
    android:layout_width="match_parent"
    android:layout_height="75px"
    app:rcb_border_color="#eee"
    app:rcb_border_padding="3px"
    app:rcb_border_width="3px"
    app:rcb_show_border="true"
    app:rcb_show_padding="true" />
```

```java
    RatioColorBar bar = (RatioColorBar) findViewById(R.id.rcb);
    List<RatioColorBar.RatioBarData> list = new ArrayList<>();
    list.add(new RatioColorBar.RatioBarData(200, 0xffd8d8d8));
    list.add(new RatioColorBar.RatioBarData(200, 0xff7ed321));
    list.add(new RatioColorBar.RatioBarData(50, 0xffd0021b));
    list.add(new RatioColorBar.RatioBarData(200, 0xff7ed321));
    list.add(new RatioColorBar.RatioBarData(10, 0xffd0021b));
    list.add(new RatioColorBar.RatioBarData(20, 0xff7ed321));
    list.add(new RatioColorBar.RatioBarData(10, 0xffd0021b));
    list.add(new RatioColorBar.RatioBarData(300, 0xfff5a623));
    list.add(new RatioColorBar.RatioBarData(200, 0xff7ed321));
    list.add(new RatioColorBar.RatioBarData(60, 0xffd8d8d8));
    list.add(new RatioColorBar.RatioBarData(100, 0xfff5a623));
    list.add(new RatioColorBar.RatioBarData(600, 0xff7ed321));
    bar.setColorBars(list);
```