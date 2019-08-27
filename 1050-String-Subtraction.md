---
title: <Python><PAT> 1050 String Subtraction
date: 2019-08-27 13:22:24
tags: 
- PAT
- 题解
- 哈希集合
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 字符串大水题
---

# 题目

Given two strings $S_{1}$ and $S_{2}, S=S_{1}-S_{2}$ is defined to be the remaining string after taking all the characters in $S_{2}$ from $S_{1}$. Your task is simply to calculate $S_{1}-S_{2}$ for any given strings. However, it might not be that simple to do it **fast**.

### Input Specification:

Each input file contains one test case. Each case consists of two lines which gives $S_1$ and $S_2$, respectively. The string lengths of both strings are no more than $10^4$. It is guaranteed that all the characters are visible ASCII codes and white space, and a new line character signals the end of a string.

### Output Specification:

For each test case, print $S_{1}-S_{2}$ in one line.

### Sample Input:

```in
They are students.
aeiou
```

### Sample Output:

```out
Thy r stdnts.
```

# 题解

## 思路

+ 对第一行字符串的每个字符，如果它不在第二行当中，那么就输出它。
+ 很显然用哈希集合。

## 数据结构

+ a是第一行，字符串
+ b是一个哈希集和，存放第二行每一个字符。

## 算法

+ 先输入第一行
+ 再输入第二行，建哈希集和
+ 对每一个第一行的字符，判断其在不在第二行，不在就输出，注意后面不加空格。

## 代码

+ 因为使用Python能够AC，因此只放了Python的代码。

```python
a = input()
b = set([i for i in input()])
for i in a:
    if i not in b:
        print(i, end='')

```

