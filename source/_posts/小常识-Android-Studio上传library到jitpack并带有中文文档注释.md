---
title: 小常识-Android Studio上传library到jitpack并带有中文文档注释
date: 2018-01-19 12:23:23
tags:
	- 常识
categories: 常识
---

### 工程的build.gradle中添加

```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.3.1'
  
        //gradle 2.2+ 使用1.4.1
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.4.1'
  
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```

<!-- more -->

### library的build.gradle中添加

- 顶部添加
```
apply plugin: 'com.github.dcendents.android-maven' // 添加这个
  
group='com.github.zhouzhuo810' // 指定group，com.github.<用户名>
```
- 底部添加
```
// 指定编码
tasks.withType(JavaCompile) {
    options.encoding = "UTF-8"
}
  
// 打包源码
task sourcesJar(type: Jar) {
    from android.sourceSets.main.java.srcDirs
    classifier = 'sources'
}
  
task javadoc(type: Javadoc) {
    failOnError  false
    source = android.sourceSets.main.java.sourceFiles
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
    classpath += configurations.compile
}
  
// 制作文档(Javadoc)
task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}
  
artifacts {
    archives sourcesJar
    archives javadocJar
}
```

### 测试一下
- 打开Android Studio 的Terminal
- 输入./gradlew install
- 如果成功就没问题了。

### 最后分享到github

### release一个版本即可

### 前往jitpack查看

- 访问jitpack官网
[https://jitpack.io](https://jitpack.io)
- 输入github仓库地址搜索
- 使用方法该页面有说明
