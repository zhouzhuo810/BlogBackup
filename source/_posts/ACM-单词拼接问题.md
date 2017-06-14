---
title: ACM-单词拼接问题
date: 2017-06-08 10:37:50
tags: 
	- ACM 
categories: ACM 
---


**描述**

给你一些单词，请你判断能否把它们首尾串起来串成一串。

前一个单词的结尾应该与下一个单词的道字母相同。

**输入**

第一行是一个整数N(0<N<20)，表示测试数据的组数
每组测试数据的第一行是一个整数M,表示该组测试数据中有M(2<M<1000)个互不相同的单词，随后的M行，每行是一个长度不超过30的单词,单词全部由小写字母组成。

**输出**

如果存在拼接方案，请输出所有拼接方案中字典序最小的方案。(两个单词之间输出一个英文句号".")
如果不存在拼接方案，则输出

```
***
```


**样例输入**

```
2
6
aloha
arachnid
dog
gopher
rat
tiger
3
oak
maple
elm
```

**样例输出**

```
aloha.arachnid.dog.gopher.rat.tiger
***
```

<!-- more -->


## 参考答案

### Java版

```java
import java.util.PriorityQueue;
import java.util.Queue;
import java.util.Scanner;
import java.util.Stack;
//欧拉通路(路径) 只有一个点的入度比出度小一, 一个点的出度比入度大一 其余点的入度与出度相等
//欧拉回路 所有点的入度与出度相等
//并查集判断连通性 只有一个点的父节点是他本身
//消圈算法遍历结果
  
class Node {
	Queue<String> queue = new PriorityQueue<String>();// 首字母相同的单词可能有多个
}
  
public class Main {
	public static final int LEN = 26;
	private int start;
	private int[] father, num, inDegree, outDegree;
	private Node[] nodes;
  
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int t, n;
		Main main;
		String str;
		Node[] nodes = new Node[LEN];
		for (int i = 0; i < LEN; i++) {
			nodes[i] = new Node();
		}
		t = sc.nextInt();
		while (t-- != 0) {
			n = sc.nextInt();
			for (int i = 0; i < n; i++) {
				str = sc.next();
				nodes[str.charAt(0) - 'a'].queue.add(str);// 单词首字母
			}
			main = new Main(nodes);
			main.execute();
			for (int i = 0; i < LEN; i++) {
				nodes[i].queue.clear();
			}
		}
		sc.close();
	}
  
	public Main(Node[] nodes) {
		this.nodes = nodes;
		father = new int[LEN];
		num = new int[LEN];
		inDegree = new int[LEN];
		outDegree = new int[LEN];
		for (int i = 0; i < LEN; i++) {
			father[i] = i;
			num[i] = 1;
		}
		for (int i = 0, start, end; i < LEN; i++) {
			for (String str : nodes[i].queue) {
				start = str.charAt(0) - 'a';// 单词首
				end = str.charAt(str.length() - 1) - 'a';// 单词尾
				inDegree[end]++;
				outDegree[start]++;
				start = find(start);
				end = find(end);
				if (start != end) {
					union(start, end);
				}
			}
		}
	}
  
	private int find(int i) {
		return i == father[i] ? i : find(father[i]);
	}
  
	private void union(int i, int j) {
		father[i] = j;
	}
  
	private boolean isEularRoute() {
		start = -1;
		int count = 0;
		boolean find = false;
		for (int i = 0; i < LEN; i++) {
			if (inDegree[i] != outDegree[i]) {
				if (Math.abs(inDegree[i] - outDegree[i]) != 1)
					return false;
				if (!find && inDegree[i] < outDegree[i]) {
					find = true;
					start = i;
				}
				if (++count > 2)
					return false;
			} else if (outDegree[i] > 0 && start == -1) {
				start = i;
			}
		}
		return true;
	}
  
	private boolean isConnected() {
		for (int i = 0, count = 0; i < LEN; i++) {
			if (outDegree[i] > 0 && father[i] == i) {
				if (++count > 1) {
					return false;
				}
			}
		}
		return true;
	}
  
	private void printResult(int i) {
		dfs(i);
		if (!stack.isEmpty())
			System.out.print(stack.pop());
		while (!stack.isEmpty()) {
			System.out.print("." + stack.pop());
		}
		System.out.println();
	}
  
	Stack<String> stack = new Stack<String>();
  
	private void dfs(int i) {
		while (!nodes[i].queue.isEmpty()) {
			String str = nodes[i].queue.poll();
			dfs(str.charAt(str.length() - 1) - 'a');
			stack.push(str);
		}
	}
  
	public void execute() {
		if (!isConnected() || !isEularRoute()) {
			System.out.println("***");
			return;
		}
		printResult(start);
	}
}
  
/**
 * 欧拉回路:若图G中存在这样一条路径，使得它恰通过G中每条边一次，则称该路径为欧拉路径。若该路径是一个圈， 则成为欧拉回路。
 * 具有欧拉回路的图成为欧拉图，具有欧拉路径但不具有欧拉回路的图成为半欧拉图。 1.无向连通图G是欧拉图，当且仅当G不含奇数度结点(G的所有结点度数为偶数)；
 * 2.无向连通图G含有欧拉通路，当且仅当G有零个或两个奇数度的结点； 3.有向连通图D是欧拉图，当且仅当该图为连通图且D中每个结点的入度=出度
 * 4.有向连通图D含有欧拉通路，当且仅当该图为连通图且D中除两个结点外， 其余每个结点的入度=出度，且此两点满足deg－(u)－deg+(v)=±1。
 * （起始点s的入度=出度-1，结束点t的出度=入度-1 或两个点的入度=出度）
 *
 * 连通图：若图中任意两点都是连通的，则该图是连通图。
 */
```