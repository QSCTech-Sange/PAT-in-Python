---
title: <Python><PAT> 1058 A+B in Hogwarts
date: 2019-08-28 15:25:59
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 大水题
---

# 题目

If you are a fan of Harry Potter, you would know the world of magic has its own currency system -- as Hagrid explained it to Harry, "Seventeen silver Sickles to a Galleon and twenty-nine Knuts to a Sickle, it's easy enough." Your job is to write a program to compute *A*+*B* where *A* and *B* are given in the standard form of `Galleon.Sickle.Knut` (`Galleon` is an integer in $\left[0,10^{7}\right]$, `Sickle` is an integer in $[0,17)$, and `Knut` is an integer in [0, 29)).

### Input Specification:

Each input file contains one test case which occupies a line with *A* and *B* in the standard form, separated by one space.

### Output Specification:

For each test case you should output the sum of *A* and *B* in one line, with the same format as the input.

### Sample Input:

```in
3.2.1 10.16.27
```

### Sample Output:

```out
14.1.28
```

# 题解

## 思路

+ 就简单的加法
+ 处理一下进位
+ 完事儿

## 数据结构

+ A,B是原数
+ S是新数

## 算法

+ 先加
+ 再计算进位。

## 代码

+ 因为使用Python能ac，因此只放了Python的代码。

```python
A, B = list(map(lambda x: x.split("."), input().split()))
S = [int(i) + int(j) for i, j in zip(A, B)]
if S[2] >= 29: 
    S[1] += 1
    S[2] -= 29
if S[1] >= 17: 
    S[0] += 1
    S[1] -= 17
print(".".join(list(map(str, S))))

```

