---
title: JavaEE-文件存储
date: 2017-09-07 11:03:10
tags:
	- JavaEE
categories: Java工具类
---


## 文件存储工具整理

- 随机名字存储
- 指定文件名存储
- 字节存储

<!-- more -->

### 工具类

```java
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;
  
import javax.servlet.http.HttpServletRequest;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.nio.charset.Charset;
  
/**
 * Created by admin on 2017/7/22.
 */
public class FileUtils {
  
    public static String saveFileToPathWithRandomName(String content, String path)
            throws IOException {
        // 创建目录
        File dir = new File(path);
        if (!dir.exists()) {
            dir.mkdirs();
        }
        // 读取文件流并保持在指定路径
        String filename = System.currentTimeMillis() + ".txt";
        String mPath = path + File.separator
                + filename;
        System.out.println(path);
        OutputStream outputStream = new FileOutputStream(mPath);
        byte[] buffer = content.getBytes(Charset.forName("utf-8"));
        outputStream.write(buffer);
        outputStream.flush();
        outputStream.close();
        return filename;
    }
  
    public static String saveFileToPathWithName(String content, String path, String filename)
            throws IOException {
        // 创建目录
        File dir = new File(path);
        if (!dir.exists()) {
            dir.mkdirs();
        }
        // 读取文件流并保持在指定路径
        String mPath = path + File.separator
                + filename;
        System.out.println(path);
        OutputStream outputStream = new FileOutputStream(mPath);
        byte[] buffer = content.getBytes(Charset.forName("utf-8"));
        outputStream.write(buffer);
        outputStream.flush();
        outputStream.close();
        return filename;
    }
  
    public static void deleteFiles(String dir) {
        File file = new File(dir);
        if (!file.exists()) {
            return;
        }
        if (file.isDirectory()) {
            File[] files = file.listFiles();
            if (files != null && files.length > 0) {
                for (File file1 : files) {
                    file1.delete();
                }
            }
        }
    }
  
    public static String saveFile(byte[] data, String dirName, String fileName) {
        HttpServletRequest request = ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes()).getRequest();
        String realPath = request.getRealPath("");
        String newpath = realPath + File.separator + dirName;
        File file = new File(newpath);
        if (!file.exists()) {
            file.mkdirs();
        }
        String filePath = newpath + File.separator + fileName;
        try {
            FileOutputStream fos = new FileOutputStream(new File(filePath), false);
            fos.write(data);
            fos.flush();
            fos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return filePath;
    }
}

```