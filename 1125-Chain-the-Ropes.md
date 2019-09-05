---
title: <Python><PAT> 1125 Chain the Ropes
date: 2019-09-05 09:20:24
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 是打结不是打劫
---

# 题目

Given some segments of rope, you are supposed to chain them into one rope. Each time you may only fold two segments into loops and chain them into one piece, as shown by the figure. The resulting chain will be treated as another segment of rope and can be folded again. After each chaining, the lengths of the original two segments will be halved.

![rope](https://images.ptausercontent.com/46293e57-aa0e-414b-b5c3-7c4b2d5201e2.jpg)

Your job is to make the longest possible rope out of *N* given segments.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer $N (2≤N≤10^4)$. Then *N* positive integer lengths of the segments are given in the next line, separated by spaces. All the integers are no more than $10^4$.

### Output Specification:

For each case, print in a line the length of the longest possible rope that can be made by the given segments. The result must be rounded to the nearest integer that is no greater than the maximum length.

### Sample Input:

```in
8
10 15 12 3 4 13 1 15
```

### Sample Output:

```out
14
```

# 题解

## 思路

+ 这题最困难的是理解题意
+ 题目的意思是，总共Ｎ个绳子，把它们连接在一起
+ 每次连接两根绳子，连起来的长度是总绳子长度的一遍，然后两根合二为一
+ （图里面完全看不出来这一点，而且也不符合常识。）
+ 总之就是两根绳子合并成一根，合并后的长度为他俩的平均数
+ 求最长的连接方式
+ 假设以一个节点为起点，不断向后连接绳子，那么这个节点的长度在总长度里会不断地除以２除以２除以２
+ 相反这样的话最后一个节点只会被除以１次２
+ 所以我们可以将所有绳子排序，然后从头开始连接，这样最长的被除以２的次数是最少的，那么最长也是最大的。

## 数据结构

+ ropes　记录所有的绳子
+ length　记录长度

## 算法

+ 排序
+ 先把最短的两个连起来
+ 不断连剩下的
+ 输出

## 代码

+ 由于使用Python可以AC，因此只放了Python的解。

```python
n = int(input())
ropes = sorted(list(map(int, input().split())))
length = (ropes[0] + ropes[1]) / 2
for i in range(2,n):
    length = (length + ropes[i])/2
print(int(length))

```

