---
title: <Python><PAT> 1065 A+B and C (64bit)
date: 2019-08-29 10:02:57
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 精度?去他的，我只要Python一把梭。
---

# 题目

Given three integers *A*, *B* and *C* in $[-2^{63},2^{63}]$, you are supposed to tell whether *A*+*B*>*C*.

### Input Specification:

The first line of the input gives the positive number of test cases, *T* (≤10). Then *T* test cases follow, each consists of a single line containing three integers *A*, *B* and *C*, separated by single spaces.

### Output Specification:

For each test case, output in one line `Case #X: true` if *A*+*B*>*C*, or `Case #X: false` otherwise, where *X* is the case number (starting from 1).

### Sample Input:

```in
3
1 2 3
2 3 4
9223372036854775807 -9223372036854775808 0
```

### Sample Output:

```out
Case #1: false
Case #2: true
Case #3: false
```

# 题解

## 思路

+ 直接算，Python没有精度范围

## 数据结构

+ a,b,c 分别对应题目里的A，B，C

## 算法

+ 直接算，就三行也没有算法可谈了。

## 代码

+ 由于使用Python能AC，因此只放了Python的代码。

```python
for i in range(int(input())):
    a, b, c = list(map(int, input().split()))
    print("Case #" + str(i + 1) + ": " + str(a + b > c).lower())
```

