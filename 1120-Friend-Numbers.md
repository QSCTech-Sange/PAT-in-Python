---
title: <Python><PAT> 1120 Friend Numbers
date: 2019-09-04 20:04:49
tags:
- PAT
- 题解
- 哈希集合
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 小case
---

# 题目

Two integers are called "friend numbers" if they share the same sum of their digits, and the sum is their "friend ID". For example, 123 and 51 are friend numbers since 1+2+3 = 5+1 = 6, and 6 is their friend ID. Given some numbers, you are supposed to count the number of different frind ID's among them.

### Input Specification

Each input file contains one test case. For each case, the first line gives a positive integer N. Then N positive integers are given in the next line, separated by spaces. All the numbers are less than $10^4$.

### Output Specification:

For each case, print in the first line the number of different frind ID's among the given integers. Then in the second line, output the friend ID's in increasing order. The numbers must be separated by exactly one space and there must be no extra space at the end of the line.

### Sample Input:

```in
8
123 899 51 998 27 33 36 12
```

### Sample Output:

```out
4
3 6 9 26
```

# 题解

## 思路

+ 照着题目要求走一遍就行了，开一个集合存储一下所有的id

## 数据结构

+ nums 是原来输入的字符串数组
+ ans是一个集合，保存所有的id

## 算法

+ 初始化数据结构
+ 对nums里的每一个数
  + 求它的id（每项字符和）
  + 将id添加到集合中
+ 打印集合长度
+ 将集合排序并输出其中内容

## 代码

+ 由于使用Python3 能AC，因此只放了Python3的题解。

```python
n = int(input())
nums = input().split()
ans = set()
for num in nums:
    ans.add(sum([int(i) for i in num]))
print(len(ans))
print(" ".join(list(map(str,sorted(ans)))))
```

