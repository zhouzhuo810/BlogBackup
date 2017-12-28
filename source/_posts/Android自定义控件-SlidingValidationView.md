---
title: Android自定义控件-SlidingValidationView
date: 2017-12-28 13:34:03
tags:
	- Android
categories: Android自定义控件
---

## 效果图

![图2](../../../../images/SlidingValidation.gif)

## 代码

### 属性
```xml
<declare-styleable name="SlidingValidationView">
    <attr name="sv_drag_view_color" format="color|reference" />
    <attr name="sv_line_color" format="color|reference" />
    <attr name="sv_final_point_color" format="color|reference" />
    <attr name="sv_sliding_progress" format="integer" />
    <attr name="sv_draw_radius" format="dimension|reference" />
    <attr name="sv_final_point_radius" format="dimension|reference" />
    <attr name="sv_line_point_radius" format="dimension|reference" />
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
import android.support.annotation.Nullable;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;
  
import com.zhy.autolayout.utils.AutoUtils;
  
/**
 * 滑动验证
 * Created by zhouzhuo810 on 2017/12/27.
 */
public class SlidingValidationView extends View {
  
    private int dragViewColor;
    private int lineColor;
    private int finalPointColor;
    private int progress;
    private boolean showDragViewShadow;
    private float slidingX;
    private int dragRadius;
    private int finalPointRadius;
    private int linePointRadius;
  
    private Paint dragViewPaint;
    private Paint leftLinePaint;
    private Paint rightLinePaint;
    private Paint finalPointPaint;
  
    private boolean isDragging;
    private boolean dragEnable;
  
    private OnSlidingListener slidingListener;
    private float downX;
  
    public interface OnSlidingListener {
        void onSlidingProgress(int progress);
  
        void onOk();
  
        void onCancel();
    }
  
    public void setSlidingListener(OnSlidingListener slidingListener) {
        this.slidingListener = slidingListener;
    }
  
    public SlidingValidationView(Context context) {
        super(context);
        init(context, null);
    }
  
    public SlidingValidationView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        init(context, attrs);
    }
  
    public SlidingValidationView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs);
    }
  
    @TargetApi(android.os.Build.VERSION_CODES.LOLLIPOP)
    public SlidingValidationView(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        init(context, attrs);
    }
  
    private void init(Context context, AttributeSet attrs) {
        if (attrs != null) {
            TypedArray t = context.obtainStyledAttributes(attrs, R.styleable.SlidingValidationView);
            dragViewColor = t.getColor(R.styleable.SlidingValidationView_sv_drag_view_color, 0xff35559b);
            lineColor = t.getColor(R.styleable.SlidingValidationView_sv_line_color, 0xff45c1f9);
            finalPointColor = t.getColor(R.styleable.SlidingValidationView_sv_final_point_color, 0xff7ed321);
            progress = t.getInteger(R.styleable.SlidingValidationView_sv_sliding_progress, 0);
            dragRadius = t.getInteger(R.styleable.SlidingValidationView_sv_draw_radius, 60);
            finalPointRadius = t.getInteger(R.styleable.SlidingValidationView_sv_final_point_radius, 18);
            linePointRadius = t.getInteger(R.styleable.SlidingValidationView_sv_line_point_radius, 6);
            t.recycle();
        } else {
            dragViewColor = 0xff35559b;
            lineColor = 0xff45c1f9;
            finalPointColor = 0xff7ed321;
            progress = 0;
            dragRadius = 60;
            finalPointRadius = 18;
            linePointRadius = 6;
        }
        dragRadius = AutoUtils.getPercentWidthSize(dragRadius);
        finalPointRadius = AutoUtils.getPercentWidthSize(finalPointRadius);
        linePointRadius = AutoUtils.getPercentWidthSize(linePointRadius);
  
        dragEnable = true;
  
        initPaint();
    }
  
    private void initPaint() {
        dragViewPaint = new Paint();
        dragViewPaint.setColor(dragViewColor);
        dragViewPaint.setStyle(Paint.Style.STROKE);
        dragViewPaint.setStrokeWidth(6f);
        dragViewPaint.setAntiAlias(true);
  
        leftLinePaint = new Paint();
        leftLinePaint.setColor(finalPointColor);
        leftLinePaint.setStyle(Paint.Style.FILL);
        leftLinePaint.setStrokeWidth(2f);
        leftLinePaint.setAntiAlias(true);
  
        rightLinePaint = new Paint();
        rightLinePaint.setColor(lineColor);
        rightLinePaint.setStyle(Paint.Style.FILL);
        rightLinePaint.setStrokeWidth(2f);
        rightLinePaint.setAntiAlias(true);
  
        finalPointPaint = new Paint();
        finalPointPaint.setColor(finalPointColor);
        finalPointPaint.setStyle(Paint.Style.FILL);
        finalPointPaint.setAntiAlias(true);
    }
  
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        drawFinalPoint(canvas);
        drawLeftLine(canvas);
        drawRightLine(canvas);
        drawDragView(canvas);
    }
  
    private void drawFinalPoint(Canvas canvas) {
        int centerX = getWidth() - dragRadius - 10;
        int centerY = dragRadius + 10;
        finalPointPaint.setStyle(Paint.Style.FILL);
        finalPointPaint.setColor(finalPointColor);
        canvas.drawCircle(centerX, centerY, finalPointRadius / 2, finalPointPaint);
        finalPointPaint.setStyle(Paint.Style.STROKE);
        finalPointPaint.setStrokeWidth(1f);
        finalPointPaint.setColor(0xff3a559b);
        canvas.drawCircle(centerX, centerY, finalPointRadius, finalPointPaint);
    }
  
    private void drawLeftLine(Canvas canvas) {
        int endX = getWidth() - dragRadius - 10;
        int startX = dragRadius + 10;
        int centerX = dragRadius + 10;
        int centerY = dragRadius + 10;
        int pointStartX = dragRadius + 10;
        int pointEndX = (int) ((progress * 1.0f / 100) * (endX - startX));
        int dx = pointEndX - pointStartX;
        int count = dx / (linePointRadius * 5);
        for (int i = 0; i < count; i++) {
            int x = pointStartX + i * dx / count;
            canvas.drawCircle(x, centerY, linePointRadius, leftLinePaint);
        }
    }
  
    private void drawRightLine(Canvas canvas) {
        int endX = getWidth() - dragRadius - 10;
        int startX = dragRadius + 10;
        int centerX = dragRadius + 10;
        int centerY = dragRadius + 10;
        int pointStartX = centerX * 2 + 20 + (int) ((progress * 1.0f / 100) * (endX - startX));
        int pointEndX = getWidth() - dragRadius - 10 - finalPointRadius;
        int dx = pointEndX - pointStartX;
        int count = dx / (linePointRadius * 5);
        for (int i = 0; i < count; i++) {
            int x = pointStartX + i * dx / count;
            canvas.drawCircle(x, centerY, linePointRadius, rightLinePaint);
        }
    }
  
    private void drawDragView(Canvas canvas) {
        int endX = getWidth() - dragRadius - 10;
        int startX = dragRadius + 10;
        int centerX = dragRadius + 10 + (int) ((progress * 1.0f / 100) * (endX - startX));
        int centerY = dragRadius + 10;
        //画圆
        dragViewPaint.setStyle(Paint.Style.STROKE);
        dragViewPaint.setAlpha((int) (255 * 0.8));
        if (progress < 90) {
            dragViewPaint.setColor(dragViewColor);
            canvas.drawCircle(centerX, centerY, dragRadius, dragViewPaint);
        } else {
            dragViewPaint.setColor(finalPointColor);
            canvas.drawCircle(centerX, centerY, dragRadius, dragViewPaint);
        }
  
        dragViewPaint.setColor(dragViewColor);
        dragViewPaint.setAlpha(255);
        if (progress < 90) {
            //画点
            dragViewPaint.setStyle(Paint.Style.FILL);
            int pointStartX = centerX - dragRadius / 2;
            int pointEndX = centerX + dragRadius / 5;
            int dx = pointEndX - pointStartX;
            for (int i = 1; i <= 4; i++) {
                int x = pointStartX + i * dx / 4;
                canvas.drawCircle(x, centerY, 3, dragViewPaint);
            }
            //画箭头
            int arrayStartX = pointEndX + dx / 2;
            int arrayEntTopY = centerY - dx / 2;
            int arrayEndBottomY = centerY + dx / 2;
            canvas.drawCircle(arrayStartX, centerY, 2, dragViewPaint);
            canvas.drawLine(arrayStartX, centerY, pointEndX, arrayEntTopY, dragViewPaint);
            canvas.drawLine(arrayStartX, centerY, pointEndX, arrayEndBottomY, dragViewPaint);
        }
    }
  
    /**
     * 还原
     */
    public void reset() {
        dragEnable = true;
        setProgress(0);
    }
  
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if (dragEnable) {
            switch (event.getAction()) {
                case MotionEvent.ACTION_DOWN:
                    int centerX = dragRadius + 10;
                    int centerY = dragRadius + 10;
                    float x = event.getX();
                    float y = event.getY();
                    double r = Math.sqrt(Math.pow((centerX - x), 2) + Math.pow((centerY - y), 2));
                    if (r < dragRadius) {
                        downX = event.getX();
                        isDragging = true;
                    }
                    break;
                case MotionEvent.ACTION_MOVE:
                    if (isDragging) {
                        float rawX = event.getX();
                        float dx = rawX - downX;
                        int endX = getWidth() - dragRadius - 10;
                        int startX = dragRadius + 10;
                        progress = (int) (dx * 100.0f / (endX - startX) + 0.5f);
                        if (progress < 0) {
                            progress = 0;
                        }
                        if (progress > 100) {
                            progress = 100;
                        }
                        if (slidingListener != null) {
                            slidingListener.onSlidingProgress(progress);
                        }
                        invalidate();
                    }
                    break;
                case MotionEvent.ACTION_UP:
                    isDragging = false;
                    if (progress > 90) {
                        progress = 100;
                        dragEnable = false;
                        if (slidingListener != null) {
                            slidingListener.onOk();
                        }
                    } else {
                        if (slidingListener != null) {
                            slidingListener.onCancel();
                        }
                        progress = 0;
                    }
                    invalidate();
                    break;
            }
        }
        return true;
    }
  
    public boolean isDragging() {
        return isDragging;
    }
  
    public boolean isDragEnable() {
        return dragEnable;
    }
  
    /**
     * 设置拖动开关
     * @param dragEnable 开／关
     */
    public void setDragEnable(boolean dragEnable) {
        this.dragEnable = dragEnable;
        invalidate();
    }
  
    public int getDragViewColor() {
        return dragViewColor;
    }
  
    public void setDragViewColor(int dragViewColor) {
        this.dragViewColor = dragViewColor;
        invalidate();
    }
  
    public int getLineColor() {
        return lineColor;
    }
  
    public void setLineColor(int lineColor) {
        this.lineColor = lineColor;
        invalidate();
    }
  
    public int getFinalPointColor() {
        return finalPointColor;
    }
  
    public void setFinalPointColor(int finalPointColor) {
        this.finalPointColor = finalPointColor;
        invalidate();
    }
  
    public boolean isShowDragViewShadow() {
        return showDragViewShadow;
    }
  
    public void setShowDragViewShadow(boolean showDragViewShadow) {
        this.showDragViewShadow = showDragViewShadow;
        invalidate();
    }
  
    public float getSlidingX() {
        return slidingX;
    }
  
    public void setSlidingX(float slidingX) {
        this.slidingX = slidingX;
        invalidate();
    }
  
    public int getProgress() {
        return progress;
    }
  
    public void setProgress(int progress) {
        this.progress = progress;
        invalidate();
    }
  
    public int getDragRadius() {
        return dragRadius;
    }
  
    public void setDragRadius(int dragRadius) {
        this.dragRadius = dragRadius;
        invalidate();
    }
  
    public int getFinalPointRadius() {
        return finalPointRadius;
    }
  
    public void setFinalPointRadius(int finalPointRadius) {
        this.finalPointRadius = finalPointRadius;
        invalidate();
    }
  
    public int getLinePointRadius() {
        return linePointRadius;
    }
  
    public void setLinePointRadius(int linePointRadius) {
        this.linePointRadius = linePointRadius;
        invalidate();
    }
  
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int width = MeasureSpec.getSize(widthMeasureSpec);
        int height = dragRadius * 2 + 20;
        setMeasuredDimension(width, height);
    }
}
```

### 使用

```xml
<!--注意包名改成自己的-->
<com.example.slidingverificationtest.ui.widget.SlidingValidationView
    android:id="@+id/sv"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="40px"
    android:layout_marginRight="40px" />
```

```java
SlidingValidationView sv = (SlidingValidationView) findViewById(R.id.sv);
sv.setOnSlidingListener(new SlidingValidationView.OnSlidingListener() {
    @Override
    public void onSlidingProgress(int progress) {
        
    }

    @Override
    public void onOk() {

    }

    @Override
    public void onCancel() {

    }
});
```