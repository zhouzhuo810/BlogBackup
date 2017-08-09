---
title: Android签名打包-指定apk文件名
date: 2017-07-27 13:03:36
tags:
	- Android
categories: Android签名打包
---


## 开发工具

- Android Studio

## 操作方法

在build.gradle中加入

<!-- more -->

```
android {
  
	//...
  
    applicationVariants.all {variant ->
        variant.outputs.each {output ->
            def outputFile = output.outputFile
            def fileName
            if (outputFile != null && outputFile.name.endsWith('.apk')) {
                if (variant.buildType.name.equals('release')) {
                    fileName = "APP名称_${defaultConfig.versionName}_${defaultConfig.versionCode}.apk"
                } else if (variant.buildType.name.equals('debug')) {
                    fileName = "APP名称_${defaultConfig.versionName}_${defaultConfig.versionCode}_debug.apk"
                }
                output.outputFile = new File(outputFile.parent, fileName)
            }
        }
    }
  
}
  
```