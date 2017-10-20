---
title: Android常用代码-微信QQ分享文件
date: 2017-10-17 15:15:30
tags:
	- Android
categories: Android常用代码
---

不使用SDK实现QQ和微信文件与文字的分享。

### QQ分享文件
```
    private void shareFileToQQ(File file) throws Exception{
        Intent share = new Intent(Intent.ACTION_SEND);
        ComponentName component = new ComponentName("com.tencent.mobileqq", "com.tencent.mobileqq.activity.JumpActivity");
        share.setComponent(component);
        Uri uri = null;
        if (Build.VERSION.SDK_INT > 23) {
            uri = FileProvider.getUriForFile(getActivity(),
                    BuildConfig.APPLICATION_ID + ".provider",
                    file);
        } else {
            uri = Uri.fromFile(file);
        }
        share.putExtra(Intent.EXTRA_STREAM, uri);
        share.setType("*/*");
        startActivity(Intent.createChooser(share, "发送"));
    }
```

<!-- more -->

### QQ分享文字

```java
    /**
     * 分享文字给QQ好友
     * @param context
     * @param content
     */
    public void shareQQ(Context context, String content) {
        Intent sendIntent = new Intent();
        sendIntent.setAction(Intent.ACTION_SEND);
        sendIntent.putExtra(Intent.EXTRA_TEXT, content);
        sendIntent.setType("text/plain");
        try {
            sendIntent.setClassName("com.tencent.mobileqq", "com.tencent.mobileqq.activity.JumpActivity");
            Intent chooserIntent = Intent.createChooser(sendIntent, "选择分享途径");
            if (chooserIntent == null) {
                return;
            }
            context.startActivity(sendIntent);
        } catch (Exception e) {
            context.startActivity(sendIntent);
        }
    }
```

### 微信分享文件

```java
    private void shareFileToWx(String title, File file) throws Exception{
        Intent intent = new Intent();
        ComponentName comp = new ComponentName("com.tencent.mm", "com.tencent.mm.ui.tools.ShareImgUI");
        intent.setComponent(comp);
        Uri uri = null;
        if (Build.VERSION.SDK_INT > 23) {
            uri = FileProvider.getUriForFile(getActivity(),
                    BuildConfig.APPLICATION_ID + ".provider",
                    file);
        } else {
            uri = Uri.fromFile(file);
        }
        intent.setAction("android.intent.action.SEND");
        intent.setType("file/*");
        intent.putExtra(Intent.EXTRA_TEXT, title);
        intent.putExtra(Intent.EXTRA_STREAM, uri);
        startActivity(intent);
    }
```

### 微信分享文字

```java
    /**
     * 分享文字给微信好友
     * @param content
     */
    private void shareToFriend(Context context, String content) {
        Intent intent = new Intent();
        intent.setAction(Intent.ACTION_SEND);
        intent.setType("text/plain");
        intent.putExtra(Intent.EXTRA_TEXT, content);
        try {
            intent.setClassName("com.tencent.mm", "com.tencent.mm.ui.tools.ShareImgUI");
            Intent chooserIntent = Intent.createChooser(intent, "选择分享途径");
            if (chooserIntent == null) {
                return;
            }
            context.startActivity(intent);
        } catch (Exception e) {
            context.startActivity(intent);
        }
    }
```

### 注意事项

- 问题1(个别手机上会报错，如'小米 REDMI NOTE 3')

```
android.content.ActivityNotFoundException
```

Unable to find explicit activity class {com.tencent.mm/com.tencent.mm.ui.tools.ShareImgUI}; have you declared this activity in your AndroidManifest.xml?
解决方式

```xml
        <activity android:name="com.tencent.mm.ui.tools.ShareImgUI"/>
        <activity android:name="com.tencent.mobileqq.activity.JumpActivity"/>
```