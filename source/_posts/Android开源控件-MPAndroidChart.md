---
title: Android开源控件-MPAndroidChart
date: 2017-06-21 18:20:41
tags:
	- Android
categories: Android开源控件
---

## 概述

### 为什么要写这个小结？

- MPAndroidChart文档是英文的，demo没有注释。
- 为了让以后使用MPAndroidChart更加方便。


<!-- more -->

## MPAndroidChart简介

### 项目地址

[https://github.com/PhilJay/MPAndroidChart](https://github.com/PhilJay/MPAndroidChart)

### 我用的版本

```
compile 'com.github.PhilJay:MPAndroidChart:v3.0.2'
```

## 用法总结

### 折线图/曲线图(LineChart)

- 初始化控件


```java
//是否画背景线
weekChart.setDrawGridBackground(false);
//是否画边界
weekChart.setDrawBorders(false);
//是否画标记
weekChart.setDrawMarkers(false);
//是否画色标
weekChart.getLegend().setEnabled(false);
//是否显示描述
weekChart.getDescription().setEnabled(false);
//空数据提示文字
weekChart.setNoDataText(getString(R.string.no_data));
//横坐标的位置(上，下，或上和下)
weekChart.getXAxis().setPosition(XAxis.XAxisPosition.BOTTOM);
//是否画横坐标的线(竖线)
weekChart.getXAxis().setDrawGridLines(false);
//是否画右边的纵坐标
weekChart.getAxisRight().setEnabled(false);
//是否画左边的纵坐标的线
weekChart.getAxisLeft().setDrawGridLines(false);
//是否画左边的纵坐标
weekChart.getAxisLeft().setDrawLabels(true);

```

- 让第一点和最后一点显示全

```java
//设置最小X坐标小于0
weekChart.getXAxis().setAxisMinimum(-0.5f);
//设置最大X坐标大于 (点的数量-0.5)
weekChart.getXAxis().setAxisMaximum(week.size()-0.5f);
```

- 设置X坐标不跳过或省略

```java
//设置X坐标个数，以保证X坐标都显示出来
weekChart.getXAxis().setLabelCount(week.size());
```

- 数据填充


```java
private void updateWeek(final List<GetDefectiveRateResult.DataEntity.WeekEntity> week) {
    if (week != null && week.size() > 0) {
        //设置左右间距
        weekChart.getXAxis().setAxisMinimum(-0.5f);
        weekChart.getXAxis().setAxisMaximum(week.size()-0.5f);
        //设置X坐标不省略
        weekChart.getXAxis().setLabelCount(week.size());
        //数据处理
        List<Entry> values = new ArrayList<>();
        for (int i = 0; i < week.size(); i++) {
            GetDefectiveRateResult.DataEntity.WeekEntity weekEntity = week.get(i);
            values.add(new Entry(i, weekEntity.getValue()));
        }
        //修改X坐标的值
        weekChart.getXAxis().setValueFormatter(new IAxisValueFormatter() {
            @Override
            public String getFormattedValue(float value, AxisBase axis) {
                return week.get((int) value).getName();
            }
        });
        
        LineDataSet lineDataSet = new LineDataSet(values, "周废品率(%)");
        //圆圈外圈的半径
        lineDataSet.setCircleRadius(4f);
        //圆圈空心的半径
        lineDataSet.setCircleHoleRadius(3f);
        //圆环的颜色
        lineDataSet.setCircleColor(getResources().getColor(R.color.colorBarColor));
        //折线或曲线的宽度
        lineDataSet.setLineWidth(2f);
        //线的模式：决定是 折线还是曲线
        lineDataSet.setMode(LineDataSet.Mode.CUBIC_BEZIER);
        //线的颜色
        lineDataSet.setColor(getResources().getColor(R.color.colorBarColor));
        //线下方是否填充颜色
        lineDataSet.setDrawFilled(true);
        //设置线下方填充的颜色
        if (Build.VERSION.SDK_INT >= 18) {
            lineDataSet.setFillDrawable(getResources().getDrawable(R.drawable.line_one_shape));
        } else {
            lineDataSet.setFillColor(0x4f2F7EDB);
        }
        LineData lineData = new LineData(lineDataSet);
        weekChart.setData(lineData);
        weekChart.invalidate();
    } else {
        //没数据时清理图表
        weekChart.clear();
    }
}
```

line_one_shape.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
    <gradient android:startColor="#7f2F7EDB" android:angle="270"
        android:endColor="@android:color/transparent"/>
</shape>
```

### 饼状图(PieChart)

- 初始化控件


```java
//空数据提示
reasonPieChart.setNoDataText(getString(R.string.no_data));
//使用百分比值画图(也就是说按数据的大小比例画图)
reasonPieChart.setUsePercentValues(true);
//描述
reasonPieChart.getDescription().setEnabled(false);
//饼状图的位置偏移
reasonPieChart.setExtraOffsets(5, 10, 5, 5);
  
//是否可以拖动旋转
reasonPieChart.setRotationEnabled(true);
//好像是拖动转圈后松手的惯性(0到1,越大越快)
reasonPieChart.setDragDecelerationFrictionCoef(0.95f);
    
//是否画中间的文字
reasonPieChart.setDrawCenterText(true);
//中间的文字
reasonPieChart.setCenterText("原因废品率(%)");
   
//是否画中间的洞
reasonPieChart.setDrawHoleEnabled(true);
//中间洞的颜色
reasonPieChart.setHoleColor(Color.WHITE);
//中间洞外边一小圈的颜色
reasonPieChart.setTransparentCircleColor(Color.WHITE);
//中间洞外边一小圈的透明度
reasonPieChart.setTransparentCircleAlpha(110);
//中间洞的半径
reasonPieChart.setHoleRadius(48f);
//洞外小圈的半径
reasonPieChart.setTransparentCircleRadius(51f);
  
//设置旋转的角度
reasonPieChart.setRotationAngle(0);
    
//是否画扇形内部的文字
reasonPieChart.setDrawEntryLabels(false);
//扇形内部文字的颜色
reasonPieChart.setEntryLabelColor(Color.BLACK);
//扇形内部文字的大小
reasonPieChart.setEntryLabelTextSize(7f);
  
    
//是否启用色标
reasonPieChart.getLegend().setEnabled(true);
Legend legend = reasonPieChart.getLegend();
//色标的方面
legend.setOrientation(Legend.LegendOrientation.VERTICAL);
//色标的水平位置
legend.setHorizontalAlignment(Legend.LegendHorizontalAlignment.RIGHT);
//色标的垂直位置
legend.setVerticalAlignment(Legend.LegendVerticalAlignment.CENTER);
//色标画在图标外面还是里面
legend.setDrawInside(false);
//色标文字的大小
legend.setTextSize(11f);
```

- 数据填充


```java
private void updateReason(final List<GetDefectiveRateResult.DataEntity.ReasonEntity> reason) {
    if (reason != null && reason.size() > 0) {
        List<PieEntry> yVals = new ArrayList<>();
        for (int i = 0; i < reason.size(); i++) {
            GetDefectiveRateResult.DataEntity.ReasonEntity reasonEntity = reason.get(i);
            //值和名称
            PieEntry barEntry = new PieEntry(reasonEntity.getValue(), "["+reasonEntity.getValue()+"%] "+reasonEntity.getName());
            yVals.add(barEntry);
        }
        PieDataSet set = new PieDataSet(yVals, "");
        ArrayList<Integer> colors = new ArrayList<>();
        //扇形的颜色
        for (int vordiplomColor : ColorTemplate.VORDIPLOM_COLORS) {
            colors.add(vordiplomColor);
        }
        for (int libertyColor : ColorTemplate.LIBERTY_COLORS) {
            colors.add(libertyColor);
        }
        set.setColors(colors);
        //数值的的颜色
        set.setValueTextColor(Color.BLACK);
        //分隔线的颜色
        set.setValueLineColor(Color.WHITE);
        PieData barData = new PieData(set);
        //数值的大小
        barData.setValueTextSize(7f);
        reasonPieChart.setData(barData);
        reasonPieChart.invalidate();
    } else {
        reasonPieChart.clear();
    }
}
```


### 柱状图(BarChart/HorizontalBarChart)

- 初始化控件

```java
//是否画柱子的底色
reasonBarChart.setDrawBarShadow(false);
//是否画数字在柱子顶部
reasonBarChart.setDrawValueAboveBar(true);
//是否显示右下角的描述
reasonBarChart.getDescription().setEnabled(false);
//是否双向指缩放
reasonBarChart.setPinchZoom(false);
//是否画背景格子
reasonBarChart.setDrawGridBackground(false);
//是否显示右边的坐标
reasonBarChart.getAxisRight().setEnabled(false);
//x轴的位置(左边，右边，两边)
reasonBarChart.getXAxis().setPosition(XAxis.XAxisPosition.BOTTOM);
//是否显示X轴的坐标线
reasonBarChart.getXAxis().setDrawGridLines(false);
//设置X轴坐标的文字大小
reasonBarChart.getXAxis().setTextSize(9f);
//设置X轴坐标的旋转角度
//reasonBarChart.getXAxis().setLabelRotationAngle(45);
//是否显示左边Y轴的坐标线
reasonBarChart.getAxisLeft().setDrawGridLines(false);
//设置左边Y轴的最小值
reasonBarChart.getAxisLeft().setAxisMinimum(0f);
//设置左边Y轴的是否显示
reasonBarChart.getAxisLeft().setEnabled(false);


//色标
Legend legend = reasonBarChart.getLegend();
legend.setEnabled(false);
legend.setOrientation(Legend.LegendOrientation.HORIZONTAL);
legend.setHorizontalAlignment(Legend.LegendHorizontalAlignment.RIGHT);
legend.setVerticalAlignment(Legend.LegendVerticalAlignment.CENTER);
legend.setForm(Legend.LegendForm.SQUARE);
legend.setFormSize(9f);
legend.setXEntrySpace(4f);
legend.setDrawInside(false);
legend.setTextSize(11f);
```

- X轴坐标转换

```java
productBarChart.getXAxis().setValueFormatter(new IAxisValueFormatter() {
    @Override
    public String getFormattedValue(float value, AxisBase axis) {
        return product.get((int) value).getName()+" ["+product.get((int) value).getValue()+"%]";
    }
});
```

- 填充数据

```java
private void updateProduct(final List<GetDefectiveRateResult.DataEntity.ProductEntity> product) {
    if (product != null && product.size() > 0) {
        //保证X坐标显示全
        productBarChart.getXAxis().setLabelCount(product.size());
        //调整显示顺序
        Collections.reverse(product);
        //整理数据
        List<BarEntry> yVals = new ArrayList<>();
        for (int i = 0; i < product.size(); i++) {
            GetDefectiveRateResult.DataEntity.ProductEntity productEntity = product.get(i);
            BarEntry barEntry = new BarEntry(i, productEntity.getValue());
            yVals.add(barEntry);
        }
        //X轴坐标转换
        productBarChart.getXAxis().setValueFormatter(new IAxisValueFormatter() {
            @Override
            public String getFormattedValue(float value, AxisBase axis) {
                return product.get((int) value).getName()+" ["+product.get((int) value).getValue()+"%]";
            }
        });
    
        BarDataSet set = new BarDataSet(yVals, "");
        //设置柱子颜色
        ArrayList<Integer> colors = new ArrayList<>();
        for (int joyfulColor : ColorTemplate.JOYFUL_COLORS) {
            colors.add(joyfulColor);
        }
        for (int colorfulColor : ColorTemplate.COLORFUL_COLORS) {
            colors.add(colorfulColor);
        }
        set.setColors(colors);
        //设置柱子上文字的颜色
        set.setValueTextColor(Color.BLACK);
        //设置柱子上文字的值是否显示
        set.setDrawValues(true);
        BarData barData = new BarData(set);
        //设置柱子上文字的大小
        barData.setValueTextSize(7f);
        productBarChart.setData(barData);
        productBarChart.invalidate();
    } else {
        productBarChart.clear();
    }
}
```

### 效果图

![图1](../../../../images/mpandroidcahrt.png)

![图2](../../../../images/mpandroidchart2.png)