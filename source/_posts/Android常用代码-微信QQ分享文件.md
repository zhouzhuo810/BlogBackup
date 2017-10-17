---
title: Android常用代码-微信QQ分享文件
date: 2017-10-17 15:15:30
tags:
	- Android
categories: Android常用代码
---

不使用SDK实现QQ和微信文件分享。

### QQ分享
```
    private void shareDbToQQ(File file) {
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

### 微信分享

```java
    private void shareToWx(File file) {
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
        intent.putExtra(Intent.EXTRA_TEXT, "CE数据库");
        intent.putExtra(Intent.EXTRA_STREAM, uri);
        startActivity(intent);
    }

```