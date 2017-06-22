---
title: Android 6.0新特性-动态权限申请
date: 2017-06-14 14:45:32
tags:
    - Android
categories: Android版本特性
---

Android 6.0之后，部分权限需要动态申请。
但是AndroidManifest.xml文件中同样需要申明。

## 常见处理方式

### 请求权限
```java
       if (Build.VERSION.SDK_INT > 22) {
            if (ActivityCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
                if (!ActivityCompat.shouldShowRequestPermissionRationale(this, Manifest.permission.WRITE_EXTERNAL_STORAGE)) {
                    Toast.makeText(MainActivity.this, "这里提示用户进入设置界面开启权限", Toast.LENGTH_SHORT).show();
                } else {
                    //Request
                    ActivityCompat.requestPermissions(MainActivity.this, new String[] {Manifest.permission.WRITE_EXTERNAL_STORAGE}, 0x01);
                }
            } else {
                //Allow...
            }
        } else {
            //Allow...
        }
```
<!-- more -->

### Activity必须implements ActivityCompat.OnRequestPermissionsResultCallback

### 处理请求结果

```java
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == 0x01) {
            if (grantResults.length > 0) {
                if (grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    Toast.makeText(MainActivity.this, "Allow", Toast.LENGTH_SHORT).show();
                    //allow...
                } else {
                    Toast.makeText(MainActivity.this, "Deny", Toast.LENGTH_SHORT).show();
                    //deny...
                }
            }
        }
    }
```

## 使用RxPermission(推荐)

[查看Github](https://github.com/tbruyelle/RxPermissions)

## 使用AndPermission

[查看Github](https://github.com/yanzhenjie/AndPermission)

