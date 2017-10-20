---
title: Java工具类-二维码生成和解析
date: 2017-10-20 15:36:31
tags:
	- JavaEE
categories: Java工具类
---

## zxing生成和解析二维码

### pom.xml
```xml
    <!-- https://mvnrepository.com/artifact/com.google.zxing/core -->
    <dependency>
        <groupId>com.google.zxing</groupId>
        <artifactId>core</artifactId>
        <version>3.3.0</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/com.google.zxing/javase -->
    <dependency>
        <groupId>com.google.zxing</groupId>
        <artifactId>javase</artifactId>
        <version>3.3.0</version>
    </dependency>
```

<!-- more -->

### 工具类

```java
import com.google.zxing.*;
import com.google.zxing.client.j2se.BufferedImageLuminanceSource;
import com.google.zxing.client.j2se.MatrixToImageWriter;
import com.google.zxing.common.BitMatrix;
import com.google.zxing.common.HybridBinarizer;
import com.google.zxing.qrcode.QRCodeWriter;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;
  
import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.nio.file.Path;
import java.util.HashMap;
import java.util.Map;
  
/**
 * 二维码解析工具类
 * Created by zz on 2017/10/20.
 */
public class QrCodeUtils {
  
    /**
     * 生成二维码Bitmap
     *
     * @param widthPix     宽
     * @param heightPix    高
     * @param content      二维码内容
     * @param fileSavePath 保存路径
     * @return 是否成功
     */
    public static boolean encodeQrCode(int widthPix, int heightPix, String content, String fileSavePath) {
        try {
            if (content == null || "".equals(content)) {
                return false;
            }
            //配置参数
            Map<EncodeHintType, Object> hints = new HashMap<>();
            hints.put(EncodeHintType.CHARACTER_SET, "utf-8");
            //容错级别
            hints.put(EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.H);
            //设置空白边距的宽度
            hints.put(EncodeHintType.MARGIN, 6); //default is 4
            // 图像数据转换，使用了矩阵转换
            BitMatrix bitMatrix = new QRCodeWriter().encode(content, BarcodeFormat.QR_CODE, widthPix, heightPix, hints);
            Path file = new File(fileSavePath).toPath();
            MatrixToImageWriter.writeToPath(bitMatrix, "png", file);
            return true;
            //必须使用compress方法将bitmap保存到文件中再进行读取。直接返回的bitmap是没有任何压缩的，内存消耗巨大！
        } catch (WriterException | IOException e) {
            e.printStackTrace();
        }
        return false;
    }
  
    /**
     * 解析二维码
     *
     * @param filePath 二维码路径
     * @return 二维码内容
     */
    public static String decodeQrCode(String filePath) {
        BufferedImage image;
        try {
            image = ImageIO.read(new File(filePath));
            LuminanceSource source = new BufferedImageLuminanceSource(image);
            Binarizer binarizer = new HybridBinarizer(source);
            BinaryBitmap binaryBitmap = new BinaryBitmap(binarizer);
            Map<DecodeHintType, Object> hints = new HashMap<DecodeHintType, Object>();
            hints.put(DecodeHintType.CHARACTER_SET, "UTF-8");
            Result result = new MultiFormatReader().decode(binaryBitmap, hints);// 对图像进行解码
            return result.getText();
        } catch (IOException | NotFoundException e) {
            e.printStackTrace();
        }
        return null;
    }
}
  
```