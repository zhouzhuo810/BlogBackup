---
title: JavaEE-SpingMVC之log4j日志配置
date: 2017-08-23 11:07:57
tags:
	- JavaEE
categories: SpringMVC
---

### web.xml 添加配置

```xml
<!-- Spring的log4j监听器 -->
<context-param>
    <param-name>webAppRootKey</param-name>
    <param-value>webapp.root1</param-value>
</context-param>
<context-param>
    <param-name>log4jConfigLocation</param-name>
    <param-value>classpath:log4j.properties</param-value>
</context-param>
<context-param>
    <param-name>log4jRefreshInterval</param-name>
    <param-value>6000</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.util.Log4jConfigListener</listener-class>
</listener>
```
<!-- more -->

### resources中添加log4j.properties

- 注意：webAppRootKey的值和${webapp.root1}的内容相对相。

```text
### 设置###
log4j.rootLogger = debug,stdout,D,E
### 输出信息到控制抬 ###
#log4j.appender.stdout = org.apache.log4j.ConsoleAppender
#log4j.appender.stdout.Target = System.out
#log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
#log4j.appender.stdout.layout.ConversionPattern = [%-5p] %d{yyyy-MM-dd HH:mm:ss,SSS} method:%l%n%m%n
### 输出DEBUG 级别以上的日志到=E://logs/error.log ###
log4j.appender.D = org.apache.log4j.DailyRollingFileAppender
log4j.appender.D.File = ${webapp.root1}/WEB-INF/logs/log.log
log4j.appender.D.Append = true
log4j.appender.D.Threshold = INFO
log4j.appender.D.layout = org.apache.log4j.PatternLayout
log4j.appender.D.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss} [%X{ip}] [ %t:%r ] - [ %p ]  %m%n
### 输出ERROR 级别以上的日志到=E://logs/error.log ###
log4j.appender.E = org.apache.log4j.DailyRollingFileAppender
log4j.appender.E.File =${webapp.root1}/WEB-INF/logs/error.log
log4j.appender.E.Append = true
log4j.appender.E.Threshold = ERROR
log4j.appender.E.layout = org.apache.log4j.PatternLayout
log4j.appender.E.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss} [%X{ip}] [ %t:%r ] - [ %p ]  %m%n
#log4j.rootLogger=INFO,CONSOLE,DAILY_ALL
#console log
#log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
#log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
#log4j.appender.CONSOLE.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} [%t] %-5p %c - %m%n
##all log
#log4j.appender.DAILY_ALL=org.apache.log4j.DailyRollingFileAppender
#log4j.appender.DAILY_ALL.layout=org.apache.log4j.PatternLayout
#log4j.appender.DAILY_ALL.layout.ConversionPattern="%p %d{yyyy-MM-dd HH:mm:ss} %-50.50c(%L) - %m%n
##${webapp.root} == the path of your tomcat path
#log4j.appender.DAILY_ALL.File=${webapp.root}/WEB-INF/logs/app.log
## General Apache libraries
#log4j.logger.org.apache=WARN
## Spring
#log4j.logger.org.springframework=WARN
# email
log4j.logger.com.alexgaoyh.util.email=INFO, email
log4j.appender.email=org.apache.log4j.DailyRollingFileAppender
log4j.appender.email.layout=org.apache.log4j.PatternLayout
log4j.appender.email.layout.ConversionPattern="%p %d{yyyy-MM-dd HH:mm:ss} %-50.50c(%L) - %m%n
log4j.appender.email.File=${webapp.root1}/WEB-INF/logs/email/email.log
```