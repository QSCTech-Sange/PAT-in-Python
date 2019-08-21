---
title: <Python><PAT> 1005 Spell It Right
date: 2019-08-19 14:26:43
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 人类迷惑行为大赏
---

Given a non-negative integer *N*, your task is to compute the sum of all the digits of *N*, and output every digit of the sum in English.

### Input Specification:

Each input file contains one test case. Each case occupies one line which contains an *N* (≤10100).

### Output Specification:

For each test case, output in one line the digits of the sum in English words. There must be one space between two consecutive words, but no extra space at the end of a line.

### Sample Input:

```in
12345
```

### Sample Output:

```out
one five
```

# 题解

## 思路

+ 迷惑行为
+ 按照它的要求走就行了
+ 使用Python处理字符串很轻松

## 数据结构

不需要

## 算法

不需要

### 代码

因为只用Python就能AC，因此只写了Python的代码。

```python
def to_english(i):
    if i == 1:return "one"
    if i == 2:return "two"
    if i == 3:return "three"
    if i == 4:return "four"
    if i == 5:return "five"
    if i == 6:return "six"
    if i == 7:return "seven"
    if i == 8:return "eight"
    if i == 9:return "nine"
    if i == 0:return "zero"


a = sum([int(i) for i in input()])
ans = []
for i in str(a):
    ans.append(to_english(int(i)))
print(" ".join(ans))

```

