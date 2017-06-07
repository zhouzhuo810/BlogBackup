---
title: ACM-(复杂排序问题)
date: 2017-06-07 16:36:38
tags: 
	- ACM 
categories: ACM 
---

**描述**

现在有很多长方形，每一个长方形都有一个编号，这个编号可以重复；还知道这个长方形的宽和长，编号、长、宽都是整数；现在要求按照一下方式排序（默认排序规则都是从小到大）；

1.按照编号从小到大排序

2.对于编号相等的长方形，按照长方形的长排序；

3.如果编号和长都相同，按照长方形的宽排序；

4.如果编号、长、宽都相同，就只保留一个长方形用于排序,删除多余的长方形；最后排好序按照指定格式显示所有的长方形；

**输入**

第一行有一个整数 0<n<10000,表示接下来有n组测试数据；
每一组第一行有一个整数 0<m<1000，表示有m个长方形；
接下来的m行，每一行有三个数 ，第一个数表示长方形的编号，

第二个和第三个数值大的表示长，数值小的表示宽，相等
说明这是一个正方形（数据约定长宽与编号都小于10000）；

**输出**

顺序输出每组数据的所有符合条件的长方形的 编号 长 宽

**样例输入**

```
1
8
1 1 1
1 1 1
1 1 2
1 2 1
1 2 2
2 1 1
2 1 2
2 2 1
```

**样例输出**

```
1 1 1
1 2 1
1 2 2
2 1 1
2 2 1
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
		int n = sc.nextInt();
		StringBuilder result = new StringBuilder();
		for (int i=0; i<n; i++) {
			List<Rect> list = new ArrayList<Rect>();
			int m = sc.nextInt();
			for (int j=0; j<m; j++) {
				int no = sc.nextInt();
				int a = sc.nextInt();
				int b = sc.nextInt();
				Rect r = new Rect(no, a>b?a:b, a<b?a:b);
				if (!isExist(list,r)) {
					list.add(r);	
				}
			}
			Collections.sort(list, new Comparator<Rect>(){  
	            @Override  
	            public int compare(Rect r1, Rect r2) {  
		            if (r1.no != r2.no) {
		            	return r1.no > r2.no ? 1 : -1;
		            } else {
		            	if (r1.a != r2.a) {
		            		return r1.a > r2.a ? 1: -1;
		            	} else {
		            		return r1.b > r2.b ? 1 : -1;
		            	} else {
		            		return 0;
		            	}
		            }
	            }  
	              
	        });  
	        for(int j=0 ; j<list.size(); j++) {
	        	result.append(list.get(j).toString()).append("\n");
	        }
		}

		System.out.println(result.toString());
	}

	public static boolean isExist(List<Rect> list, Rect r) {
		for (int i=0; i<list.size(); i++) {
			Rect r1 = list.get(i);
			if (r1.no == r.no && r1.a==r.a && r1.b == r.b) {
				return true;
			}
		}
		return false;
	}

	public static class Rect {
		public int no;
		public int a;
		public int b;

		public Rect(int no, int a, int b) {
			this.no = no;
			this.a = a;
			this.b = b;
		}

		public String toString() {
			return no + " " + a +" " + b ;
		}
	}
}
```