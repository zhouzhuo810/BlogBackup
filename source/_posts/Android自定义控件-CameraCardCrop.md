---
title: Android自定义控件-CameraCardCrop
date: 2017-06-22 17:31:54
tags:
	- Android
categories: Android自定义控件
---

# CameraCardCrop
一个卡片（证件）拍照裁剪框架。
(A cutting framework for card-photo.)

## Gradle

```
compile 'me.zhouzhuo810.cameracardcrop:camera-card-crop:1.0.2'
```

<!-- more -->

## Screenshot

<img src="../../../../images/cameracrop2.png"  width="300px"/>
<img src="../../../../images/cameracrop1.png"  width="300px"/>

## Notice

```
card

---------------------
|       width       |
|                   |
|                   |height
|                   |
---------------------
phone
------------------------------------
|                                   |
|                                   |
|                                   |
|                                   |
|                                   |
|      mask                         |
|                                   |
|                width              |
|    ------------------------       |
|    |                       |      |
|    |                height |      | screen height
|    |         rect          |      |
|    |                       |      |
|    ------------------------       |
|                                   |
|                                   |
|                                   |
|                                   |
|                                   |
|            screen width           |
-------------------------------------
CameraConfig.RATIO_WIDTH = card's width
CameraConfig.RATIO_HEIGHT = card's height
CameraConfig.PERCENT_WIDTH = rect'swidth / screen's width
```

## Usage

### step 1. Add Activity in your AndroidManifest.xml file.

```xml
    <activity android:name="me.zhouzhuo810.cameracardcrop.CropActivity"
        android:screenOrientation="portrait"
        android:theme="@style/Theme.AppCompat.NoActionBar">
    </activity>
```

### step 2. Add permissions in your AndroidManifest.xml file.

```xml
    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/>

    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />
```

### step 3. Example for use.


```java
    public void takePhoto(View v) {
        Intent intent = new Intent(MainActivity.this, CropActivity.class);
        intent.putExtra(CameraConfig.RATIO_WIDTH, 855);
        intent.putExtra(CameraConfig.RATIO_HEIGHT, 541);  
        intent.putExtra(CameraConfig.PERCENT_WIDTH, 0.8f); //[0,1]
        intent.putExtra(CameraConfig.MASK_COLOR, 0x2f000000);
        intent.putExtra(CameraConfig.RECT_CORNER_COLOR, 0xff00ff00);
        intent.putExtra(CameraConfig.TEXT_COLOR, 0xffffffff);
        intent.putExtra(CameraConfig.HINT_TEXT, "请将方框对准证件拍照");
        intent.putExtra(CameraConfig.IMAGE_PATH, Environment.getExternalStorageDirectory().getAbsolutePath()+"/CameraCardCrop/"+System.currentTimeMillis()+".jpg");
        startActivityForResult(intent, 0x01);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK) {
            if (requestCode == 0x01) {
                String path = data.getStringExtra(CameraConfig.IMAGE_PATH);
                ivPic.setImageURI(Uri.parse("file://"+path));
            }
        }
    }

```
