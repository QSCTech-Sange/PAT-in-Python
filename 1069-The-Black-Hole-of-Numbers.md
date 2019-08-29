---
title: <Python><PAT> 1069 The Black Hole of Numbers
date: 2019-08-29 19:47:35
tags: 
- PAT
- 题解
- Python
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 遇到字符串就用Python吧！
---

# 题目

For any 4-digit integer except the ones with all the digits being the same, if we sort the digits in non-increasing order first, and then in non-decreasing order, a new number can be obtained by taking the second number from the first one. Repeat in this manner we will soon end up at the number `6174` -- the **black hole** of 4-digit numbers. This number is named Kaprekar Constant.

For example, start from `6767`, we'll get:

```
7766 - 6677 = 1089
9810 - 0189 = 9621
9621 - 1269 = 8352
8532 - 2358 = 6174
7641 - 1467 = 6174
... ...
```

Given any 4-digit number, you are supposed to illustrate the way it gets into the black hole.

### Input Specification:

Each input file contains one test case which gives a positive integer *N* in the range (0,104).

### Output Specification:

If all the 4 digits of *N* are the same, print in one line the equation `N - N = 0000`. Else print each step of calculation in a line until `6174` comes out as the difference. All the numbers must be printed as 4-digit numbers.

### Sample Input 1:

```in
6767
```

### Sample Output 1:

```out
7766 - 6677 = 1089
9810 - 0189 = 9621
9621 - 1269 = 8352
8532 - 2358 = 6174
```

### Sample Input 2:

```in
2222
```

### Sample Output 2:

```out
2222 - 2222 = 0000
```

# 题解

## 思路

+ 题目不是很难，主要是字符串的处理不要忘了0
+ 就是当我们计算出两数只差时，要对不足四位数的前面补足0,再参与下一轮的排序并做差。
+ 这点使用Python很方便
+ `c = "0" * (4 - len(str(a - b))) + str(a - b)`即可
+ 同时a和b的更新，是对c进行排序。Python排序后会返回一个列表，要拼接转为字符串。并且做差之前要再转为整数。
+ `b = int("".join(sorted(c)))` 即可

## 数据结构

+ a 是被减数
+ b 是减数
+ c 是差

## 算法

+ 减法

## 代码

+ 由于使用Python能AC，因此只放了Python的代码。

```python
temp = input()
c = "0" * (4 - len(temp)) + temp
a, b = int("".join(sorted(c, reverse=True))), int("".join(sorted(c)))
while True:
    a, b = int("".join(sorted(c, reverse=True))), int("".join(sorted(c)))
    c = "0" * (4 - len(str(a - b))) + str(a - b)
    print("%04d - %04d = %s" % (a, b, c))
    if c == "6174" or c == "0000":
        break

```

