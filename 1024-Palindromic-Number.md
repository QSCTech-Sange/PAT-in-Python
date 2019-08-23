---
title: <Python><PAT> 1024 Palindromic Number
date: 2019-08-22 20:22:43
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 字符串的题就全用Python吧。我不要你觉得，我要我觉得
---

# 题目

A number that will be the same when it is written forwards or backwards is known as a **Palindromic Number**. For example, 1234321 is a palindromic number. All single digit numbers are palindromic numbers.

Non-palindromic numbers can be paired with palindromic ones via a series of operations. First, the non-palindromic number is reversed and the result is added to the original number. If the result is not a palindromic number, this is repeated until it gives a palindromic number. For example, if we start from 67, we can obtain a palindromic number in 2 steps: 67 + 76 = 143, and 143 + 341 = 484.

Given any positive integer *N*, you are supposed to find its paired palindromic number and the number of steps taken to find it.

### Input Specification:

Each input file contains one test case. Each case consists of two positive numbers *N* and *K*, where *N* (≤1010) is the initial numer and *K* (≤100) is the maximum number of steps. The numbers are separated by a space.

### Output Specification:

For each test case, output two numbers, one in each line. The first number is the paired palindromic number of *N*, and the second number is the number of steps taken to find the palindromic number. If the palindromic number is not found after *K* steps, just output the number obtained at the *K*th step and *K* instead.

### Sample Input 1:

```in
67 3
```

### Sample Output 1:

```out
484
2
```

### Sample Input 2:

```in
69 3
```

### Sample Output 2:

```out
1353
3
```

# 题解

## 思路

+ 照着题目走就行了，好像不需要什么思路

## 数据结构

+ num 原数字
+ max_step 最大步数
+ step 已进行步数s

## 算法

+ 回文直接使用字符串回一下完事儿。
+ 别的就不需要什么算法了

## 代码

因为使用Python3能AC，因此只写了Python的代码。

```python
num, max_step = list(map(int, input().split()))
step = 0
while step < max_step:
    if str(num) == str(num)[::-1]:
        print(num)
        print(step)
        break
    num += int(str(num)[::-1])
    step += 1
else:
    print(num)
    print(step)

```

