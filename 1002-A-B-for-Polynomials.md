---
title: <Python><PAT> 1002 A+B for Polynomials
date: 2019-08-19 11:11:33
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 大水题
---

# 题目

This time, you are supposed to find *A*+*B* where *A* and *B* are two polynomials.

### Input Specification:

Each input file contains one test case. Each case occupies 2 lines, and each line contains the information of a polynomial:
$$
K\;N_1\; a_{N_1} \;N_2\; a_{N_2} ... \;N_k\; a_{N_k}
$$
where $K$ is the number of nonzero terms in the polynomial, $N_i$ and $a_{N_i}(i=1,2,⋯,K)$ are the exponents and coefficients, respectively. It is given that $1≤K≤10，0≤N_K<⋯<N_2<N_1≤1000.$

### Output Specification:

For each test case you should output the sum of *A* and *B* in one line, with the same format as the input. Notice that there must be NO extra space at the end of each line. Please be accurate to 1 decimal place.

### Sample Input:

```in
2 1 2.4 0 3.2
2 2 1.5 1 0.5
```

### Sample Output:

```out
3 2 1.5 1 2.9 0 3.2
```

# 题解

## 思路

+ 理解题意
+ 题意是，给两个多项式让你相加
+ 就是指数相同的要把系数相加
+ 输入格式是，先给你项数，然后一个指数，一个系数，一个指数，一个系数……这样整两行
+ 输出格式同样，指数按从大到小排列。
+ 一开始想到用哈希表来做
+ 但是因为结果要排序，怪麻烦的
+ 算了，数组一把梭

## 代码

### 数据结构

+ poly是一个数组
  + 下标代表指数
  + 值代表系数

### 算法

就20行代码，还有近一半是空行，我也不知道该怎么说算法了，一看代码就懂。

### 代码本体

```python
poly = [0 for _ in range(1001)]

def add():
    global poly
    line = input().split()[1:]
    i = 0
    while i < len(line) - 1:
        poly[int(line[i])] += float(line[i + 1])
        i += 2

add()
add()

count = 0
for i in range(1000,-1,-1):
    if poly[i] != 0:
        count += 1

print(count, end='')
for i in range(1000, -1, -1):
    if poly[i] != 0:
        print(" %d %.1f" % (i, poly[i]), end='')
```

