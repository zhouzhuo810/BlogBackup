---
title: ACM-(素数求和问题)
date: 2017-06-07 15:20:35
tags: 
	- ACM 
categories: ACM 
---

**描述**

现在给你N个数（0<N<1000），现在要求你写出一个程序，找出这N个数中的所有素数，并求和。

**输入**

第一行给出整数M(0<M<10)代表多少组测试数据
每组测试数据第一行给你N，代表该组测试数据的数量。
接下来的N个数为要测试的数据，每个数小于1000

**输出**

每组测试数据结果占一行，输出给出的测试数据的所有素数和

**样例输入**


```
3
5
1 2 3 4 5
8
11 12 13 14 15 16 17 18
10
21 22 23 24 25 26 27 28 29 30
```

**样例输出**

```
10
41
52
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
		int zu = sc.nextInt();
		int[] sum = new int[zu];
		for(int i=0; i<zu; i++) {
			int count = sc.nextInt();
			int s = 0;
			for(int j=0; j<count; j++) {
				int value = sc.nextInt();
				if (isPrime(value)) {
					s += value;
				}
			}
			sum[i] = s;
		} 
		for(int i=0; i<zu; i++) {
			System.out.println(sum[i]);
		}
	}

    public static boolean isPrime(int a) {  
        boolean flag = true;  
        if (a < 2) {// 素数不小于2  
            return false;  
        } else {  
            for (int i = 2; i <= Math.sqrt(a); i++) {  
                if (a % i == 0) {// 若能被整除，则说明不是素数，返回false  
                    flag = false;  
                    break;// 跳出循环  
                }  
            }  
        }  
        return flag;  
    }  
}
```
