---
title: <Python><PAT> 1132 Cut Integer
date: 2019-09-05 18:02:29
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 憨憨题
---

# 题目

Cutting an integer means to cut a K digits lone integer Z into two integers of (K/2) digits long integers A and B. For example, after cutting Z = 167334, we have A = 167 and B = 334. It is interesting to see that Z can be devided by the product of A and B, as 167334 / (167 × 334) = 3. Given an integer Z, you are supposed to test if it is such an integer.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤ 20). Then N lines follow, each gives an integer $Z (10 ≤ Z <2^{31})$. It is guaranteed that the number of digits of Z is an even number.

### Output Specification:

For each case, print a single line `Yes` if it is such a number, or `No` if not.

### Sample Input:

```in
3
167334
2333
12345678
```

### Sample Output:

```out
Yes
No
No
```

# 题解

## 思路

+ 挺憨的，照着题目做就行了
+ 只要处理好除数为0的特殊情况就可以了。

## 数据结构

+ 不用写了吧

## 算法

+ 不用写了吧

## 代码

+ 由于使用Python可以AC，因此只放了Python的题解。

```python
n = int(input())
for _ in range(n):
    num = input()
    a = int(num[:len(num) // 2])
    b = int(num[len(num) // 2:])
    if a != 0 and b != 0 and int(num) %(a * b) == 0:
        print("Yes")
    else:
        print("No")

```

