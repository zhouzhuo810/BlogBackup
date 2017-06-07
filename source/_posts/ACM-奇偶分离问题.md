---
title: ACM-(奇偶分离问题)
date: 2017-06-07 15:46:07
tags: 
	- ACM 
categories: ACM 
---


**描述**

有一个整型偶数n(2<= n <=10000),你要做的是：先把1到n中的所有奇数从小到大输出，再把所有的偶数从小到大输出。

**输入**

第一行有一个整数i（2<=i<30)表示有 i 组测试数据；
每组有一个整型偶数n。

**输出**

第一行输出所有的奇数
第二行输出所有的偶数

**样例输入**

```
2
10
14
```

**样例输出**

```
1 3 5 7 9 
2 4 6 8 10 

1 3 5 7 9 11 13 
2 4 6 8 10 12 14
```

<!-- more -->

## 参考答案

### Java版


```java
import java.io.*;
import java.util.*;
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int count = sc.nextInt();
		String[] result = new String[count];
		for(int i=0; i<count; i++) {
			int n = sc.nextInt();
			StringBuilder sbj = new StringBuilder();
			StringBuilder sbo = new StringBuilder();
			for (int j=1; j<=n; j++) {
				if (isOval(j)) {
					sbo.append(j).append(" ");
				} else {
					sbj.append(j).append(" ");
				}
			}
			result[i] = sbj.toString()+"\n"+sbo.toString();
		}

		for (int i=0; i<count; i++) {
			System.out.println(result[i]);
		}
	}

	public static boolean isOval(int i) {
		return i%2==0;
	}
}
```
