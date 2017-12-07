---
title: Android签名打包-多渠道打包
date: 2017-12-07 13:59:28
tags:
	- Android
categories: Android签名打包
---

### 复制libs

- common
- analytics

### 集成友盟+ App统计功能

在Application的onCreate方法中调用
```java
        /**
         * 初始化common库
         * 参数1:上下文，不能为空
         * 参数2:设备类型，UMConfigure.DEVICE_TYPE_PHONE为手机、UMConfigure.DEVICE_TYPE_BOX为盒子，默认为手机
         * 参数3:Push推送业务的secret
         */
        try {
            UMConfigure.init(this, UMConfigure.DEVICE_TYPE_PHONE, "你的appID");
        } catch (Exception e) {
            e.printStackTrace();
        }

```

### 添加相应权限

```xml
<!-- 必须的权限 -->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
  
<!-- 推荐的权限 -->
<!-- 添加如下权限，以便使用更多的第三方SDK和更精准的统计数据 -->
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### 配置渠道

```xml
<manifest>
    <application ……>
        ……
        <!--umeng start-->
        <meta-data android:value="你的AppID" android:name="UMENG_APPKEY"/>
        <meta-data android:value="${UMENG_CHANNEL_VALUE}" android:name="UMENG_CHANNEL"/>
        <!--umeng end-->
    </application>    
</manifest>
```

### 代码混淆

```
keep class com.umeng.commonsdk.** {*;}
```


### 多渠道配置

```
android {
    productFlavors {
        wandoujia {}
        yingyongbao {}
        c360 {}
        kq {}
        productFlavors.all { flavor ->
            flavor.manifestPlaceholders = [UMENG_CHANNEL_VALUE: name]
        }
    }
  
    applicationVariants.all { variant ->
        variant.outputs.each { output ->
            def outputFile = output.outputFile
            def fileName
            def flavor = productFlavors.name;
            if (outputFile != null && outputFile.name.endsWith('.apk')) {
                if (variant.buildType.name.equals('release')) {
                    fileName = "名字_${defaultConfig.versionName}_${flavor}.apk"
                } else if (variant.buildType.name.equals('debug')) {
                    fileName = "名字_${defaultConfig.versionName}_debug_${flavor}.apk"
                }
                output.outputFile = new File(outputFile.parent, fileName)
            }
        }
    }
}
```