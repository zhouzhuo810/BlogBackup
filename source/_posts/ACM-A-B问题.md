---
title: ACM-(A+B问题)
date: 2017-06-07 14:35:15
tags: 
	- ACM 
categories: ACM 
---

**描述**

此题为练手用题，请大家计算一下a+b的值.

**输入**

输入两个数，a,b

**输出**

输出a+b的值

**样例输入**

```
2 3
```


样例输出

```
5
```

<!-- more -->

## 参考答案

### C语言版

```C
#include<stdio.h>
int main()
{
int a,b;
scanf("%d%d",&a,&b);
printf("%d\n",a+b);
}
```


### Java版


```java
import java.io.*;
import java.util.*;
public class Main
{
public static void main(String args[]) throws Exception
{
Scanner cin=new Scanner(System.in);
int a=cin.nextInt(),b=cin.nextInt();
System.out.println(a+b);
}
}
```
