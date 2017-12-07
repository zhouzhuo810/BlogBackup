---
title: Android常用代码-bugly集成
date: 2017-12-07 14:05:40
tags:
	- Android
categories: Android常用代码
---

## 集成bugly

```
    //bugly
    compile 'com.tencent.bugly:crashreport:latest.release'
```

## 添加权限

```xml
    <!-- bugly start-->
    <uses-permission android:name="android.permission.READ_LOGS" />
    <!--bugly end-->
```

## Application的onCreate方法中初始化


### 不带X5内核

```java
        try {
            CrashReport.initCrashReport(this, "你的appID", false);
        } catch (Exception e) {
            e.printStackTrace();
        }
```

### 带X5内核


```java
        /*bugly+x5内核*/
        try {
            CrashReport.UserStrategy strategy = new CrashReport.UserStrategy(this);
            strategy.setCrashHandleCallback(new CrashReport.CrashHandleCallback() {
                public Map onCrashHandleStart(int crashType, String errorType, String errorMessage, String errorStack) {
                    LinkedHashMap map = new LinkedHashMap();
                    String x5CrashInfo = com.tencent.smtt.sdk.WebView.getCrashExtraMessage(GFApplication.this);
                    map.put("x5crashInfo", x5CrashInfo);
                    return map;
                }
  
                @Override
                public byte[] onCrashHandleStart2GetExtraDatas(int crashType, String errorType, String errorMessage, String errorStack) {
                    try {
                        return "Extra data.".getBytes("UTF-8");
                    } catch (Exception e) {
                        return null;
                    }
                }
            });
            CrashReport.initCrashReport(this, "你的appID", false, strategy);
        } catch (Exception e) {
            e.printStackTrace();
        }
```