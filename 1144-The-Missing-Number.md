---
title: <Python><PAT> 1144 The Missing Number
date: 2019-09-06 15:37:13
tags:
- PAT
- 题解
- 哈希集合
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 哈希集合的简单应用
---

# 题目

Given N integers, you are supposed to find the smallest positive integer that is NOT in the given list.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer $N (≤10^5)$. Then N integers are given in the next line, separated by spaces. All the numbers are in the range of **int**.

### Output Specification:

Print in a line the smallest positive integer that is missing from the input list.

### Sample Input:

```in
10
5 -25 9 6 1 3 4 2 5 17
```

### Sample Output:

```out
7
```

# 题解

## 思路

+ 题目的意思是
+ 给你一串数
+ 让你从1,2,3,4,5一直到正无穷，找到第一个不在列表里的数
+ 那么只要建立哈希集合，然后照着步骤来就可以了。

## 数据结构

+ nums 是一个哈希集和，存放所有的数

## 算法

+ 没啥算法

## 代码

+ 由于使用Python能够AC，因此只放了Python的题解。

```python
_ = int(input())
nums = set(list(map(int,input().split())))
for i in range(1,9999999999):
    if i not in nums:
        print(i)
        break
```

