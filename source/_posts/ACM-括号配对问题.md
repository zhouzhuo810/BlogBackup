---
title: ACM-(括号配对问题)
date: 2017-06-07 14:41:38
tags: 
	- ACM 
categories: ACM 
---

**描述**

现在，有一行括号序列，请你检查这行括号是否配对。

**输入**

第一行输入一个数N（0<N<=100）,表示有N组测试数据。后面的N行输入多组输入数据，每组输入数据都是一个字符串S(S的长度小于10000，且S不是空串），测试数据组数少于5组。数据保证S中只含有"[","]","(",")"四种字符

**输出**

每组输入数据的输出占一行，如果该字符串中所含的括号是配对的，则输出Yes,如果不配对则输出No

**样例输入**
```
3
[(])
(])
([[]()])
```

**样例输出**
```
No
No
Yes
```
<!-- more -->

## 参考答案

### Java版


```java
import java.io.*;
import java.util.*;
public class Main {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int count= sc.nextInt();
        String[] strs = new String[count];
        for(int i=0; i<count; i++) {
            strs[i] = sc.next();
        }
        Stack<Character> stack = null;  
        for(int j=0; j<count; j++) {
            String str = strs[j];
            if(str.length() % 2 == 1){  
                System.out.println("No");  
            }else{    
                stack = new Stack<Character>();  
                for(int i=0;i<str.length();i++){  
                    if(stack.isEmpty()){  
                        stack.push(str.charAt(i));  
                    }else if(stack.peek() == '[' && str.charAt(i) == ']' || stack.peek() == '(' && str.charAt(i) == ')'){   
                        stack.pop();  
                    }else{  
                        stack.push(str.charAt(i));  
                    }  
                }  
                if(stack.isEmpty()){  
                    //如果栈是空的，说明括号匹配  
                    System.out.println("Yes");  
                }else{  
                    //说明栈不为空，括号不匹配  
                    System.out.println("No");  
                }  
            }
        }
    }
}
```

