---
title: Android疑难杂症-串口通讯
date: 2017-08-23 11:04:21
tags:
	- Android
categories: Android疑难杂症
---

[usb-serial-for-android](https://github.com/mik3y/usb-serial-for-android)

在这个项目的基础上我将串口通信的功能封装了起来，用起来更加简单了。

Github传送门：[OkUSB](https://github.com/zhouzhuo810/OkUSB)

# OkUSB
一个简洁的Android串口通信框架。

## 功能简介

- 支持设置波特率
- 支持设置数据位
- 支持设置停止位
- 支持设置校验位
- 支持DTS和RTS
- 支持串口连接状态监听


<!-- more -->

##　用法简介

### Gradle

```
    allprojects {
        repositories {
            ...
            maven { url 'https://jitpack.io' }
        }
    }

    dependencies {
            compile 'com.github.zhouzhuo810:OkUSB:1.0.0'
    }


```

### 具体用法

1.在AndroidManifest.xml中添加如下特性：

```xml
    <uses-feature android:name="android.hardware.usb.host" />
```

在需要连接串口的Activity中添加：

```xml
    <intent-filter>
        <action android:name="android.hardware.usb.action.USB_DEVICE_ATTACHED" />
    </intent-filter>
    <meta-data
        android:name="android.hardware.usb.action.USB_DEVICE_ATTACHED"
        android:resource="@xml/device_filter" />
```

2.在res文件夹创建xml文件夹，新建device_filter.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- 要进行通信的USB设备的供应商ID（VID）和产品识别码（PID）-->
    <usb-device vendor-id="0403" product-id="6001" />
</resources>
```

3.前面都是准备工作，这里才真正使用。

```java
  
    //初始化
    usb = new USB.USBBuilder(this)
            .setBaudRate(115200)  //波特率
            .setDataBits(8)       //数据位
            .setStopBits(UsbSerialPort.STOPBITS_1) //停止位
            .setParity(UsbSerialPort.PARITY_NONE)  //校验位
            .setMaxReadBytes(20)   //接受数据最大长度
            .setReadDuration(500)  //读数据间隔时间
            .setDTR(false)    //DTR enable
            .setRTS(false)    //RTS enable
            .build();
  
    //实现监听连接状态和数据收发。
    usb.setOnUsbChangeListener(new USB.OnUsbChangeListener() {
        @Override
        public void onUsbConnect() {
            Toast.makeText(MainActivity.this, "conencted", Toast.LENGTH_SHORT).show();
        }
  
        @Override
        public void onUsbDisconnect() {
            Toast.makeText(MainActivity.this, "disconencted", Toast.LENGTH_SHORT).show();
        }
  
        @Override
        public void onDataReceive(byte[] data) {
            tvResult.setText(new String(data, Charset.forName("GB2312")));
        }
  
        @Override
        public void onUsbConnectFailed() {
            Toast.makeText(MainActivity.this, "connect fail", Toast.LENGTH_SHORT).show();
        }
  
        @Override
        public void onPermissionGranted() {
            Toast.makeText(MainActivity.this, "permission ok", Toast.LENGTH_SHORT).show();
        }
  
        @Override
        public void onPermissionRefused() {
            Toast.makeText(MainActivity.this, "permission fail", Toast.LENGTH_SHORT).show();
        }
  
        @Override
        public void onDriverNotSupport() {
            Toast.makeText(MainActivity.this, "no driver", Toast.LENGTH_SHORT).show();
        }
  
        @Override
        public void onWriteDataFailed(String error) {
            Toast.makeText(MainActivity.this, "write fail" + error, Toast.LENGTH_SHORT).show();
        }
  
        @Override
        public void onWriteSuccess(int num) {
            Toast.makeText(MainActivity.this, "write ok " + num, Toast.LENGTH_SHORT).show();
        }
    });
  
```

4.发送数据

```java
        if (usb != null) {
            usb.destroy();
        }
```

5.如果是Activity，可以在onDestroy中调用如下代码关闭串口。

```java

        if (usb != null) {
            usb.destroy();
        }

```
