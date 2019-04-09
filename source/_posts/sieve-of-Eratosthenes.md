---
title: 埃拉托斯特尼筛法(sieve of Eratosthenes)——Java实现
comments: false
date: 2019-04-09 19:12:51
tags: Math
---

> **埃拉托斯特尼筛法**（[希腊语](https://zh.wikipedia.org/wiki/%E5%B8%8C%E8%87%98%E8%AA%9E)：κόσκινον Ἐρατοσθένους，英语：sieve of Eratosthenes ），简称**埃氏筛**，也称**素数筛**。这是一种简单且历史悠久的[筛法](https://zh.wikipedia.org/wiki/%E7%AD%9B%E6%B3%95)，用来找出一定范围内所有的[素数](https://zh.wikipedia.org/wiki/%E8%B3%AA%E6%95%B8)。
>
> 所使用的原理是从2开始，将每个[素数](https://zh.wikipedia.org/wiki/%E8%B3%AA%E6%95%B8)的各个倍数，标记成[合数](https://zh.wikipedia.org/wiki/%E5%90%88%E6%95%B8)。一个素数的各个倍数，是一个差为此素数本身的等差数列。此为这个筛法和[试除法](https://zh.wikipedia.org/wiki/%E8%A9%A6%E9%99%A4%E6%B3%95)不同的关键之处，后者是以素数来测试每个待测数能否被整除。
>
> 埃拉托斯特尼筛法是列出所有小素数最有效的方法之一，其名字来自于[古希腊数学家](https://zh.wikipedia.org/wiki/%E5%8F%A4%E5%B8%8C%E8%87%98%E6%95%B8%E5%AD%B8)[埃拉托斯特尼](https://zh.wikipedia.org/wiki/%E5%9F%83%E6%8B%89%E6%89%98%E6%96%AF%E7%89%B9%E5%B0%BC)，并且被描述在另一位古希腊数学家[尼科马库斯](https://zh.wikipedia.org/wiki/%E5%B0%BC%E7%A7%91%E9%A9%AC%E5%BA%93%E6%96%AF)所著的《算术入门》中。                                  ——wiki中文

### 解释

详细看wiki[步骤](<https://zh.wikipedia.org/wiki/%E5%9F%83%E6%8B%89%E6%89%98%E6%96%AF%E7%89%B9%E5%B0%BC%E7%AD%9B%E6%B3%95#%E6%AD%A5%E9%A9%9F>)

本篇根据wiki示例的C++代码给出Java实现

### 算法伪代码：

```
Input: an integer n > 1
 
Let A be an array of Boolean values, indexed by integers 2 to n,
initially all set to true.
 
 for i = 2, 3, 4, ..., not exceeding √n 
  if A[i] is true:
    for j = i2, i2+i, i2+2i, i2+3i, ..., not exceeding n :
      A[j] := false
 
Output: all i such that A[i] is true.
```

### 程序实现：

#### Java

```java
import java.util.Arrays;
public class Eratosthenes {
	public static void main(String[] args) {
        //给出一个0~23的数列的范围
		eratoshenes(23);
	}
	static void eratoshenes(int max){
		boolean[] flag = new boolean[max+1];
        //使用java.util.Arrays包提供的fill()方法进行快速填充初始化
		Arrays.fill(flag, true);
        //0不是质数
		flag[0] = false;
        //1不是质数
		flag[1] = false;
		for (int i = 2; i <= max; i++) {
			//当i为质数时，i的所有倍数都不是质数
			if (flag[i]) {
				for (int j = i*i; j <= max; j+=i) {
					flag[j] = false;
				}
			}
		}
		// 打印范围内的全部素数
		for (int i = 0; i <= max; i++) {
			if (flag[i]) {
				System.out.print(i +"\t");			
			}
		}
	}
}
```

#### 结果：

```
2	3	5	7	11	13	17	19	23	
```

