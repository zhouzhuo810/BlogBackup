---
title: Android 7.0新特性-FileUriExposedException
date: 2017-06-14 13:30:53
tags:
	- Android
categories: Android版本特性
---

下面两个问题都是同一个错误FileUriExposedException。

## 问题1：App更新

最近有客户反应App自动更新安装失败。
查看原因报的是FileUriExposedException错误。

查看官网API找到了解决之法。

<!-- more -->

### 新建res/xml/provider_paths.xml文件

```xhtml
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <external-path
        name="external_files"
        path="." />

    <root-path
        name="root_path"
        path="." />
        
</paths>
```

### 在AndroidManifest.xml中添加provider

```xhtml
<provider
    android:name="android.support.v4.content.FileProvider"
    android:authorities="您的包名.provider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/provider_paths"/>
</provider>
```


### 修改Uri的获取方式

```java
private void installApk(String fileName) {
    Intent intent = new Intent(Intent.ACTION_VIEW);
    if (Build.VERSION.SDK_INT > 23) {
        //FIX ME by ZZ : 7.0
        Uri uri = FileProvider.getUriForFile(MainActivity.this, BuildConfig.APPLICATION_ID+".provider", 
        		new File(Constants.APK_DOWNLOAD_DIR + File.separator + fileName));
        //这flag很关键
        intent.setFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION|Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
        intent.setDataAndType(uri, "application/vnd.android.package-archive");
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        this.startActivity(intent);
    } else {
        intent.setDataAndType(Uri.fromFile(new File(Constants.APK_DOWNLOAD_DIR + File.separator + fileName)),
                "application/vnd.android.package-archive");
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        this.startActivity(intent);
    }
}
```


## 问题2：调用系统相机拍照


### 解决方式1

（1）前两个步骤同上

（2）

```java
private void takePhoto() {
    File file = new File(PATH);
    if (!file.isDirectory()) {
        file.mkdirs();
    }
    Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
    Uri uri;
    if (Build.VERSION.SDK_INT > 23) {
        uri = FileProvider.getUriForFile(MainActivity.this,
                BuildConfig.APPLICATION_ID+".provider",
                new File(PATH+NAME));
        intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION|Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
    } else {
        uri = Uri.fromFile(new File(PATH+NAME));
    }
    intent.putExtra(MediaStore.EXTRA_OUTPUT, uri);
    startActivityForResult(intent, 0x01);
}
```
	

**注意别漏了这句:**

```java
	intent.setFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION|Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
```

### 解决方式2

```java
private void takePhoto() {
    final File file = new File(Constants.PIC_UPLOAD_ROOT_PATH);
    if (!file.isDirectory()) {
        file.mkdirs();
    }
    Intent cameraIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);//构造intent
    Uri uri;
    String pathOne = Constants.PIC_UPLOAD_ROOT_PATH + System.currentTimeMillis() + ".jpg";
    AbSharedUtil.putString(this, Constants.PUT_FILE_ONE, pathOne);
    final File fileOne = new File(pathOne);
    if (Build.VERSION.SDK_INT<24){
        uri = Uri.fromFile(fileOne);
    }else {
        ContentValues contentValues = new ContentValues(1);
        contentValues.put(MediaStore.Images.Media.DATA, fileOne.getAbsolutePath());
        uri = getContentResolver().insert(MediaStore.Images.Media.EXTERNAL_CONTENT_URI,contentValues);
    }
    if (fileOne.exists()) {
        fileOne.delete();
    }
    cameraIntent.putExtra(MediaStore.EXTRA_OUTPUT, uri);
    startActivityForResult(cameraIntent, CAMERA_REQUEST_ONE);//发出intent，并要求返回调用结果
}
```