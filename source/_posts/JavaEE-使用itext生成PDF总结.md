---
title: JavaEE-使用itext生成PDF总结
date: 2017-08-18 17:11:05
tags:
	- JavaEE
categories: SpringMVC
---

### 添加pom依赖

```xml
<!-- pdf start-->
<dependency>
    <groupId>com.itextpdf</groupId>
    <artifactId>itextpdf</artifactId>
    <version>5.5.11</version>
</dependency>
<dependency>
    <groupId>com.itextpdf</groupId>
    <artifactId>itext-asian</artifactId>
    <version>5.2.0</version>
</dependency>
<!-- pdf end -->
```
<!-- more -->

## 基本功能

### 添加普通文字(不换行)

```java
private void addText(Document document, String text, Font font) {
    if (font == null) {
        Chunk chunk = new Chunk(text == null ? "" : text);
        try {
            document.add(chunk);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
    } else {
        font.setSize(12f);
        font.setStyle(Font.NORMAL);
        Chunk chunk = new Chunk(text == null ? "" : text, font);
        try {
            document.add(chunk);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
    }
}
```

### 添加普通文字(换行)

```java
private void addTextLine(Document document, String text, Font font) {
    if (font == null) {
        Chunk chunk = new Chunk((text == null ? "" : text) + "\n");
        try {
            document.add(chunk);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
    } else {
        font.setSize(12f);
        font.setStyle(Font.NORMAL);
        Chunk chunk = new Chunk((text == null ? "" : text) + "\n", font);
        try {
            document.add(chunk);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
    }
}
```

### 添加下划线文字(带超链接)

```java
/**
 * 添加下划线(带超链接)
 *
 * @param document Document对象
 * @param text     链接文字
 * @param fontPath 字体路径
 * @throws IOException       异常
 * @throws DocumentException 异常
 */
private void addUnderLineText(Document document, String text, String fontPath) throws IOException, DocumentException {
    Font font = new Font(BaseFont.createFont(fontPath + "Arial.ttf", BaseFont.IDENTITY_H, BaseFont.NOT_EMBEDDED), 12f, Font.UNDERLINE);
    font.setColor(BaseColor.BLUE);
    Anchor anchor = new Anchor(text, font);
    anchor.setReference(text);
    anchor.setName(text);
    document.add(anchor);
}
```

### 添加表格

- 方法封装

```java
  
/**
 * 添加表格
 *
 * @param document     Document对象
 * @param tableTitle   表格标题
 * @param titles       标题
 * @param widthRatio   标题宽的比例
 * @param values       内容
 * @param titleBgColor 标题行背景颜色
 * @param font         字体
 * @throws DocumentException 异常
 */
private List<PdfPRow> addTable(Document document, String tableTitle, String[] titles, float[] widthRatio, String[][] values, BaseColor titleBgColor, Font font) throws DocumentException {
    //添加表格标题
    font.setSize(14f);
    font.setStyle(Font.BOLD);
    Chunk paragraph = new Chunk(tableTitle == null ? "" : tableTitle, font);
    try {
        document.add(paragraph);
    } catch (DocumentException e) {
        e.printStackTrace();
    }
    font.setSize(12f);
    font.setStyle(Font.NORMAL);
    //添加表格
    PdfPTable inter = new PdfPTable(titles.length);
    inter.setWidthPercentage(100); // 宽度100%填充
    inter.setSpacingBefore(1f); // 前间距
    inter.setSpacingAfter(1f); // 后间距
    List<PdfPRow> listRow = inter.getRows(); //行
    //设置列宽比例
    inter.setWidths(widthRatio);
    //标题
    PdfPCell cells[] = new PdfPCell[titles.length];
    PdfPRow titleRow = new PdfPRow(cells);
    //单元格
    for (int i = 0; i < titles.length; i++) {
        cells[i] = new PdfPCell(new Paragraph(titles[i], font));
        cells[i].setBackgroundColor(titleBgColor == null ? BaseColor.LIGHT_GRAY : titleBgColor);
        cells[i].setPaddingTop(6f);
        cells[i].setPaddingBottom(6f);
    }
    //把第一行添加到集合
    listRow.add(titleRow);
    for (String[] value : values) {
        PdfPCell childCells[] = new PdfPCell[value.length];
        PdfPRow row = new PdfPRow(childCells);
        //添加列
        for (int i = 0; i < value.length; i++) {
            childCells[i] = new PdfPCell(new Paragraph(value[i], font));//单元格内容
            childCells[i].setPaddingTop(4f);
            childCells[i].setPaddingBottom(4f);
        }
        //添加行
        listRow.add(row);
    }
    document.add(inter);
    return listRow;
}
```

- 用法示例

```java
private void pdfAddRequestHeaders(Document document, String projectId, String id, Font font) throws DocumentException {
    List<RequestHeaderEntity> globals = mRequestHeaderService.getGlobalRequestHeaders(projectId);
    List<RequestHeaderEntity> args = mRequestHeaderService.getBaseDao().executeCriteria(ResponseArgUtils.getArgByInterfaceId(id));
  
    if (globals != null && globals.size() > 0) {
        //设置列宽
        float[] columnWidths = {3f, 2f, 5f};
        String[][] values = new String[args.size()][3];
        for (int i = 0; i < args.size(); i++) {
            RequestHeaderEntity entity = args.get(i);
            values[i][0] = entity.getName();
            values[i][1] = entity.getValue() == null ? "" : entity.getValue();
            values[i][2] = entity.getNote() == null ? "" : entity.getNote();
        }
        addTable(document, "全局请求头", new String[]{"名称", "默认值", "说明"}, columnWidths, values, null, font);
    }
  
    if (args != null && args.size() > 0) {
        //设置列宽
        float[] columnWidths = {3f, 2f, 5f};
        String[][] values = new String[args.size()][3];
        for (int i = 0; i < args.size(); i++) {
            RequestHeaderEntity entity = args.get(i);
            values[i][0] = entity.getName();
            values[i][1] = entity.getValue() == null ? "" : entity.getValue();
            values[i][2] = entity.getNote() == null ? "" : entity.getNote();
        }
        addTable(document, "其他请求头", new String[]{"名称", "默认值", "说明"}, columnWidths, values, null, font);
    }
}
```

## 难点

### 添加页码

- 使用

```java
PdfWriter pdfWriter = PdfWriter.getInstance(document, new FileOutputStream(filePath));
PdfReportM1HeaderFooter footer = new PdfReportM1HeaderFooter();
pdfWriter.setPageEvent(footer);
//打开文档
document.open();
//... ...
```

- 工具类

```java
/**
 * Project Name:report
 * File Name:PdfReportM1HeaderFooter.java
 * Package Name:com.riambsoft.report.pdf
 * Date:2013-9-16上午08:59:00
 * Copyright (c) 2013, riambsoft All Rights Reserved.
 */
  
import java.io.IOException;
  
import com.itextpdf.text.Document;
import com.itextpdf.text.DocumentException;
import com.itextpdf.text.Element;
import com.itextpdf.text.Font;
import com.itextpdf.text.PageSize;
import com.itextpdf.text.Phrase;
import com.itextpdf.text.Rectangle;
import com.itextpdf.text.pdf.BaseFont;
import com.itextpdf.text.pdf.ColumnText;
import com.itextpdf.text.pdf.PdfContentByte;
import com.itextpdf.text.pdf.PdfPageEventHelper;
import com.itextpdf.text.pdf.PdfTemplate;
import com.itextpdf.text.pdf.PdfWriter;
  
public class PdfReportM1HeaderFooter extends PdfPageEventHelper {
  
    /**
     * 页眉
     */
    public String header = "";
  
    /**
     * 文档字体大小，页脚页眉最好和文本大小一致
     */
    public int presentFontSize = 12;
  
    /**
     * 文档页面大小，最好前面传入，否则默认为A4纸张
     */
    public Rectangle pageSize = PageSize.A4;
  
    // 模板
    public PdfTemplate total;
  
    // 基础字体对象
    public BaseFont bf = null;
  
    // 利用基础字体生成的字体对象，一般用于生成中文文字
    public Font fontDetail = null;
  
    /**
     *
     * Creates a new instance of PdfReportM1HeaderFooter 无参构造方法.
     *
     */
    public PdfReportM1HeaderFooter() {
    }
  
    /**
     *
     * Creates a new instance of PdfReportM1HeaderFooter 构造方法.
     *
     * @param yeMei
     *            页眉字符串
     * @param presentFontSize
     *            数据体字体大小
     * @param pageSize
     *            页面文档大小，A4，A5，A6横转翻转等Rectangle对象
     */
    public PdfReportM1HeaderFooter(String yeMei, int presentFontSize, Rectangle pageSize) {
        this.header = yeMei;
        this.presentFontSize = presentFontSize;
        this.pageSize = pageSize;
    }
  
    public void setHeader(String header) {
        this.header = header;
    }
  
    public void setPresentFontSize(int presentFontSize) {
        this.presentFontSize = presentFontSize;
    }
  
    /**
     *
     * TODO 文档打开时创建模板
     *
     * @see com.itextpdf.text.pdf.PdfPageEventHelper#onOpenDocument(com.itextpdf.text.pdf.PdfWriter, com.itextpdf.text.Document)
     */
    public void onOpenDocument(PdfWriter writer, Document document) {
        total = writer.getDirectContent().createTemplate(70, 70);// 共 页 的矩形的长宽高
    }
  
    /**
     *
     * TODO 关闭每页的时候，写入页眉，写入'第几页共'这几个字。
     *
     * @see com.itextpdf.text.pdf.PdfPageEventHelper#onEndPage(com.itextpdf.text.pdf.PdfWriter, com.itextpdf.text.Document)
     */
    public void onEndPage(PdfWriter writer, Document document) {
  
        try {
            if (bf == null) {
                bf = BaseFont.createFont("STSong-Light", "UniGB-UCS2-H", false);
            }
            if (fontDetail == null) {
                fontDetail = new Font(bf, presentFontSize, Font.NORMAL);// 数据体字体
            }
        } catch (DocumentException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
  
        // 1.写入页眉
        ColumnText.showTextAligned(writer.getDirectContent(), Element.ALIGN_LEFT, new Phrase(header, fontDetail), document.left(), document.top() + 20, 0);
  
        // 2.写入前半部分的 第 X页/共
        int pageS = writer.getPageNumber();
        String foot1 = "第 " + pageS + " 页 /共";
        Phrase footer = new Phrase(foot1, fontDetail);
  
        // 3.计算前半部分的foot1的长度，后面好定位最后一部分的'Y页'这俩字的x轴坐标，字体长度也要计算进去 = len
        float len = bf.getWidthPoint(foot1, presentFontSize);
  
        // 4.拿到当前的PdfContentByte
        PdfContentByte cb = writer.getDirectContent();
  
        //自己增加的
/*        if (pageS == 1) {
            Phrase footerLeft = new Phrase("", fontDetail);
            ColumnText.showTextAligned(cb, Element.ALIGN_LEFT, footerLeft, document.left(), document.bottom() - 20, 0);
        }*/
        // 5.写入页脚1，x轴就是(右margin+左margin + right() -left()- len)/2.0F 再给偏移20F适合人类视觉感受，否则肉眼看上去就太偏左了 ,y轴就是底边界-20,否则就贴边重叠到数据体里了就不是页脚了；注意Y轴是从下往上累加的，最上方的Top值是大于Bottom好几百开外的。
        ColumnText.showTextAligned(cb, Element.ALIGN_CENTER, footer, (document.rightMargin() + document.right() + document.leftMargin() - document.left() - len) / 2.0F + 20F, document.bottom() - 20, 0);
        // 6.写入页脚2的模板（就是页脚的Y页这俩字）添加到文档中，计算模板的和Y轴,X=(右边界-左边界 - 前半部分的len值)/2.0F + len ， y 轴和之前的保持一致，底边界-20
        cb.addTemplate(total, (document.rightMargin() + document.right() + document.leftMargin() - document.left()) / 2.0F + 20F, document.bottom() - 20); // 调节模版显示的位置
    }
  
    /**
     *
     * TODO 关闭文档时，替换模板，完成整个页眉页脚组件
     *
     * @see com.itextpdf.text.pdf.PdfPageEventHelper#onCloseDocument(com.itextpdf.text.pdf.PdfWriter, com.itextpdf.text.Document)
     */
    public void onCloseDocument(PdfWriter writer, Document document) {
        // 7.最后一步了，就是关闭文档的时候，将模板替换成实际的 Y 值,至此，page x of y 制作完毕，完美兼容各种文档size。
        total.beginText();
        total.setFontAndSize(bf, presentFontSize);// 生成的模版的字体、颜色
        String foot2 = " " + (writer.getPageNumber()) + " 页";
        total.showText(foot2);// 模版显示的内容
        total.endText();
        total.closePath();
    }
}
```

### 添加书签

```java
/**
 * 添加分组标题(带标签)
 * @param document Document对象
 * @param text 标题内容
 * @param number 标题数字
 * @param font 字体
 * @return Chapter对象
 */
private Chapter addGroup(Document document, String text, int number, Font font) {
    addTextLine(document, "", font);
    addTextLine(document, "", font);
    if (font != null) {
        font.setSize(22f);
        font.setStyle(Font.BOLD);
        Paragraph paragraph = new Paragraph( (text == null ? "" : text), font);
        Chapter chapter = new Chapter(paragraph, number);
        chapter.setBookmarkOpen(true);
        chapter.setBookmarkTitle(number + ". " + text);
        chapter.setTriggerNewPage(false);
        try {
            document.add(chapter);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
        return chapter;
    } else {
        Paragraph paragraph = new Paragraph((text == null ? "" : text));
        Chapter chapter = new Chapter(paragraph, number);
        chapter.setBookmarkOpen(true);
        chapter.setBookmarkTitle(number + ". " + text);
        chapter.setTriggerNewPage(false);
        try {
            document.add(paragraph);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
        return chapter;
    }
}
  
  
/**
 * 添加组内小标题(带标签)
 *
 * @param document Document对象
 * @param chapter  Chapter对象
 * @param text     标题
 * @param number   序号
 * @param font     字体
 */
private void addGroupItem(Document document, Chapter chapter, String text, int groupNumber, int number, Font font) {
    addTextLine(document, "", font);
    if (font != null) {
        font.setSize(18f);
        font.setStyle(Font.BOLD);
        Paragraph paragraph = new Paragraph((text == null ? "" : text), font);
        Section section = chapter.addSection(paragraph);
        section.setBookmarkOpen(true);
        section.setBookmarkTitle(groupNumber + "." + number + ". " + text);
        section.setTriggerNewPage(false);
        section.setNumberStyle(Section.NUMBERSTYLE_DOTTED_WITHOUT_FINAL_DOT);
        try {
            document.add(section);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
    } else {
        Paragraph paragraph = new Paragraph((text == null ? "" : text));
        Section section = chapter.addSection(paragraph);
        section.setBookmarkOpen(true);
        section.setBookmarkTitle(groupNumber + "." + number + ". " + text);
        section.setNumberStyle(Section.NUMBERSTYLE_DOTTED_WITHOUT_FINAL_DOT);
        section.setTriggerNewPage(false);
        try {
            document.add(section);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
    }
}
```

### 添加水印

```java
/**
 * 【功能描述：添加文字水印】
 *
 * @param srcFile    待加水印文件
 * @param destFile   加水印后存放地址
 * @param text       加水印的文本内容
 * @param textWidth  文字横坐标 推荐300-400
 * @param textHeight 文字纵坐标 推荐300-400
 * @throws Exception
 */
public String addWaterMark(String srcFile, String destFile, String text,
                           int textWidth, int textHeight) throws Exception {
    // 待加水印的文件
    PdfReader reader = new PdfReader(srcFile);
    // 加完水印的文件
    PdfStamper stamper = new PdfStamper(reader, new FileOutputStream(
            destFile));
    int total = reader.getNumberOfPages() + 1;
    PdfContentByte content;
    // 设置字体
    BaseFont font = BaseFont.createFont();
    // 循环对每页插入水印
    for (int i = 1; i < total; i++) {
        // 水印的起始
        content = stamper.getUnderContent(i);
        // 开始
        content.beginText();
        // 设置颜色 默认为浅灰色
        content.setColorFill(new BaseColor(238, 238, 238));
        // 设置字体及字号
        content.setFontAndSize(font, 50);
        // 设置起始位置
        // content.setTextMatrix(400, 880);
        content.setTextMatrix(textWidth, textHeight);
        // 开始写入水印
        content.showTextAligned(Element.ALIGN_CENTER, text, textWidth,
                textHeight, 45);
        content.endText();
    }
    stamper.close();
    return destFile;
}
```