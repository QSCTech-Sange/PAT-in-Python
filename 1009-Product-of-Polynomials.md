---
title: <Python><PAT> 1009 Product of Polynomials
date: 2019-08-19 19:17:33
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 中到易的题
---

# 题目

This time, you are supposed to find *A*×*B* where *A* and *B* are two polynomials.

### Input Specification:

Each input file contains one test case. Each case occupies 2 lines, and each line contains the information of a polynomial:
$$
K\; N_1\; a_{N_1}\; N_2 \;a_{N_2}\; ... \;N_K\; a_{N_K}
$$
where *K* is the number of nonzero terms in the polynomial, $N_i$ and $a_i{N_i} (i=1,2,⋯,K)$ are the exponents and coefficients, respectively. It is given that $1≤K≤10, 0≤N_K<⋯<N_2<N_1≤1000$.

### Output Specification:

For each test case you should output the product of *A* and *B* in one line, with the same format as the input. Notice that there must be **NO** extra space at the end of each line. Please be accurate up to 1 decimal place.

### Sample Input:

```in
2 1 2.4 0 3.2
2 2 1.5 1 0.5
```

### Sample Output:

```out
3 3 3.6 2 6.0 1 1.6
```

# 题解

## 思路

+ 先把第一个行列式存起来
+ 读取第二个行列式的时候，对每一项，都要和第一项的每一项相乘
+ 其中，指数相加，系数相称
+ 原理很简单，主要是数据结构
+ 优雅一点可以采用哈希集，暴力一点可以采用数组
+ 这题时间限制很宽松，使用数组问题不大
+ 我对第一个行列式使用了哈希集，对答案使用了数组，两种都给大家cover到。

## 数据结构

+ poly 是一个哈希表，用以存放第一个行列式
  + 键是指数
  + 值是系数
+ ans 是一个数组，用以存放答案
  + 下标代表指数
  + 值代表系数，默认是0

## 算法

+ 对第一个行列式读取并存放到哈希集合中
+ 读取第二个行列式的每一项，将每一项都和原来的哈希集的每一项相乘，添加到答案中
+ 输出数量和答案

## 代码

因为使用Python能AC，因此只放了Python AC的题解。

```python
# 数据结构定义
poly = dict()
ans = [0 for _ in range(2001)]

# 第一行输入
a = input().split()[1:]
i = 0
while i < len(a):
    poly[int(a[i])] = float(a[i + 1])
    i += 2
    
# 第二行输入并计算
a = input().split()[1:]
i = 0
while i < len(a):
    for j, k in poly.items():
        expo = j + int(a[i])
        coef = float(a[i + 1]) * k
        ans[expo] += coef
    i += 2

# 输出count
count = 0
for i in ans:
    if i != 0:
    	count += 1
print(count, end='')

# 输出多项式
for i in range(2000, -1, -1):
    if ans[i] != 0:
        print(" %d %.1f" % (i, ans[i]), end='')
```

