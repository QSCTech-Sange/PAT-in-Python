---
title: <Python><PAT> 1073 Scientific Notation
date: 2019-08-30 22:33:16
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 看到字符串，请使用——
---

# 题目

Scientific notation is the way that scientists easily handle very large numbers or very small numbers. The notation matches the regular expression [+-][1-9]`.`[0-9]+E[+-][0-9]+ which means that the integer portion has exactly one digit, there is at least one digit in the fractional portion, and the number and its exponent's signs are always provided even when they are positive.

Now given a real number *A* in scientific notation, you are supposed to print *A* in the conventional notation while keeping all the significant figures.

### Input Specification:

Each input contains one test case. For each case, there is one line containing the real number *A* in scientific notation. The number is no more than 9999 bytes in length and the exponent's absolute value is no more than 9999.

### Output Specification:

For each test case, print in one line the input number *A* in the conventional notation, with all the significant figures kept, including trailing zeros.

### Sample Input 1:

```in
+1.23400E-03
```

### Sample Output 1:

```out
0.00123400
```

### Sample Input 2:

```in
-1.2E+10
```

### Sample Output 2:

```out
-12000000000
```

# 题解

## 思路

+ 这道题给我们一个科学计数法，让我们计算出原来的值
+ 看似简单，但实际上格式条件有点难判断的
+ 核心是找到小数点的位置。
+ 我们把E前后分为两部分，前面称为real，后面称为expo（即指数）
+ real部分不包含开头的正负号和小数点
+ expo是正数，代表往后移动几位，负数代表往前移动几位，而我们小数点一开始的位置是1不是0。
+ 所以`int(expo) + 1`就代表了我们小数点应该去的位置。
+ 在`int(expo) + 1`是正数，且后面不需要补0 的情况下，直接插入在那个位置插入0就好了。比如说1.23 × 10变成12.3，小数点后移1位。
+ 但是要注意，如果`int(expo) + 1`超过了real本身的长度，那么就需要再后面补0,补到小数点的位置。
+ 如果`int(expo) + 1`是负数呢？
+ 那么就需要在前面补`0.000`，后面0的个数正好是`-(int(expo) + 1)`
+ 这个可以自己重复试几次，就不会错了。
+ 本身靠自己硬要理的话，是比较绕。

## 数据结构

+ real是原数的E之前部分
+ expo是原数E之后部分
+ ans是存放答案的，是一个字符数组。

## 算法

+ 参见思路

## 代码

+ 由于使用Python能AC，因此只放了Python的代码。

```python
real, expo = input().split('E')
res = [i for i in real if i not in "+-."]
expo = int(expo) + 1
if expo > 0:
    if expo >= len(res):
        res = res + ['0' * (expo - len(res))]
    else:
        res.insert(expo, ".")
else:
    res = ['0.', '0' * (-expo)] + res
if real[0] == '-':
    res = ['-'] + res
print("".join(res))

```

