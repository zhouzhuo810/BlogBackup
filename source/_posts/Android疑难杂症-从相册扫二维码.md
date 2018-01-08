---
title: Android疑难杂症-从相册扫二维码
date: 2018-01-08 08:54:21
tags:
	- Android
categories: Android疑难杂症
---

### 导入zxing包

```
    //zxing
    compile 'com.google.zxing:core:3.2.1'
```

<!-- more -->

### 核心代码

```java
private String scanAlbum(String path) {
    if (TextUtils.isEmpty(path)) {
        return null;
    }
    // DecodeHintType 和EncodeHintType
    Hashtable<DecodeHintType, String> hints = new Hashtable<>();
    hints.put(DecodeHintType.CHARACTER_SET, "utf-8"); // 设置二维码内容的编码
    BitmapFactory.Options options = new BitmapFactory.Options();
    options.inJustDecodeBounds = false; // 获取新的大小
    int sampleSize = (int) (options.outHeight / (float) 200);
    if (sampleSize <= 0)
        sampleSize = 1;
    options.inSampleSize = sampleSize;
    Bitmap scanBitmap = BitmapFactory.decodeFile(path, options);
    int[] intArray = new int[scanBitmap.getWidth() * scanBitmap.getHeight()];
    scanBitmap.getPixels(intArray, 0, scanBitmap.getWidth(), 0, 0, scanBitmap.getWidth(), scanBitmap.getHeight());
    RGBLuminanceSource source = new RGBLuminanceSource(scanBitmap.getWidth(), scanBitmap.getHeight(), intArray);
    BinaryBitmap bitmap1 = new BinaryBitmap(new HybridBinarizer(source));
    QRCodeReader reader = new QRCodeReader();
    try {
        Result decode = reader.decode(bitmap1, hints);
        return recode(decode.toString());
    } catch (NotFoundException | com.google.zxing.FormatException | ChecksumException e) {
        e.printStackTrace();
    }
    return null;
}

private String recode(String str) {
    String formart = "";
    try {
        boolean ISO = Charset.forName("ISO-8859-1").newEncoder().canEncode(str);
        if (ISO) {
            formart = new String(str.getBytes("ISO-8859-1"), "GB2312");
        } else {
            formart = str;
        }
    } catch (UnsupportedEncodingException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }
    return formart;
}
```

### 调用

```java
showPd(getString(R.string.scaning_text), false);
Observable.just(filePath)
        .map(new Func1<String, String>() {
            @Override
            public String call(String s) {
                return scanAlbum(s);
            }
        })
        .compose(RxHelper.<String>io_main())
        .subscribe(new Action1<String>() {
            @Override
            public void call(String s) {
                hidePd();
                if (s != null) {
                    ToastUtils.showCustomBgToast(s);
                }
            }
        }, new Action1<Throwable>() {
            @Override
            public void call(Throwable throwable) {
                throwable.printStackTrace();
                hidePd();
            }
        });
```