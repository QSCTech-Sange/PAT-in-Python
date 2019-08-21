---
title: <Python><PAT> 1019 General Palindromic Number
date: 2019-08-21 17:57:06
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 这道题让你见识到Python多屌
---

# 题目

A number that will be the same when it is written forwards or backwards is known as a **Palindromic Number**. For example, 1234321 is a palindromic number. All single digit numbers are palindromic numbers.

Although palindromic numbers are most often considered in the decimal system, the concept of palindromicity can be applied to the natural numbers in any numeral system. Consider a number *N*>0 in base *b*≥2, where it is written in standard notation with $k+1$ digits $a_i$ as $\Sigma_{i=0}^{k}(a_ib^i)$). Here, as usual, $0\le a_i < b$ for all $i$ and $a_k$ is non-zero. Then $N$ is palindromic if and only if $a_i=a_{k-i}$ for all $i$. Zero is written 0 in any base and is also palindromic by definition.

Given any positive decimal integer *N* and a base *b*, you are supposed to tell if *N* is a palindromic number in base *b*.

### Input Specification:

Each input file contains one test case. Each case consists of two positive numbers *N* and *b*, where $0<N\le 10^9$ is the decimal number and $2\le b \le 10^9$ is the base. The numbers are separated by a space.

### Output Specification:

For each test case, first print in one line `Yes` if *N* is a palindromic number in base *b*, or `No` if not. Then in the next line, print *N* as the number in base *b* in the form "$a_ka_{k-1}...a_0$". Notice that there must be no extra space at the end of output.

### Sample Input 1:

```in
27 2
```

### Sample Output 1:

```out
Yes
1 1 0 1 1
```

### Sample Input 2:

```in
121 5
```

### Sample Output 2:

```out
No
4 4 1
```

**鸣谢网友“CCPC拿不到牌不改名”修正数据！**

# 题解

## 思路

+ 就是问你一个十进制数转为其他进制后
+ 它是不是回文
+ 用Python一步登天，爽爆

## 数据结构

+ ans数组是转换进制后的数，每一项代表一位

## 算法

+ 将一个十进制数转为任意进制数的方法
  + 新建一个ans代表新数
  + 将原数与进制的模放到首
  + 原数等于原数//进制
  + 当原数等于0的时候停止循环
+ 回文直接使用列表逆序

## 代码

因为使用Python能AC，因此只有Python代码。

```python
num, radix = list(map(int, input().split()))
ans = []
while num != 0:
    ans.append(str(num % radix))
    num //= radix
if ans != ans[::-1]: print("No")
else:print("Yes")
print(" ".join(ans[::-1]))
```

