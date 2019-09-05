---
title: <Python><PAT> 1136 A Delayed Palindrome
date: 2019-09-05 20:16:38
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 回文字符串
---

# 题目

Consider a positive integer *N* written in standard notation with *k*+1 digits $a_i$ as $a_k \cdots a_1a_0$ with $0≤a_i<10$ for all *i* and $a_k>0$. Then *N* is **palindromic** if and only if  $a_i = a_{k-i}$ for all $i$. Zero is written 0 and is also palindromic by definition.

Non-palindromic numbers can be paired with palindromic ones via a series of operations. First, the non-palindromic number is reversed and the result is added to the original number. If the result is not a palindromic number, this is repeated until it gives a palindromic number. Such number is called **a delayed palindrome**. (Quoted from https://en.wikipedia.org/wiki/Palindromic_number )

Given any positive integer, you are supposed to find its paired palindromic number.

### Input Specification:

Each input file contains one test case which gives a positive integer no more than 1000 digits.

### Output Specification:

For each test case, print line by line the process of finding the palindromic number. The format of each line is the following:

```
A + B = C
```

where `A` is the original number, `B` is the reversed `A`, and `C` is their sum. `A` starts being the input number, and this process ends until `C` becomes a palindromic number -- in this case we print in the last line `C is a palindromic number.`; or if a palindromic number cannot be found in 10 iterations, print `Not found in 10 iterations.` instead.

### Sample Input 1:

```in
97152
```

### Sample Output 1:

```out
97152 + 25179 = 122331
122331 + 133221 = 255552
255552 is a palindromic number.
```

### Sample Input 2:

```in
196
```

### Sample Output 2:

```out
196 + 691 = 887
887 + 788 = 1675
1675 + 5761 = 7436
7436 + 6347 = 13783
13783 + 38731 = 52514
52514 + 41525 = 94039
94039 + 93049 = 187088
187088 + 880781 = 1067869
1067869 + 9687601 = 10755470
10755470 + 07455701 = 18211171
Not found in 10 iterations.
```

# 题解

## 思路

+ 照着要求反转字符串就行了
+ 注意输出要以字符串的形式
+ 不然前面的0会被忽略掉

## 数据结构

+ 没啥数据结构

## 算法

+ 开始十次遍历
  + 每次遍历前把字符串反转一下
  + 如果反转后和原字符串是相等的
    + 输出相等的那段文字并break
  + 否则，求它们俩的和
  + 输出
  + 使原数等于刚刚的和的字符串的表示
+ 十次都没有则输出没找到。

## 代码

+ 由于使用Python可以ac，因此只放了Python的解。

```python
a = input()
for i in range(10):
    b = a[::-1]
    if b == a:
        print(a, "is a palindromic number.")
        break
    sums = int(a) + int(b)
    print(a, "+", b, "=", sums)
    a = str(sums)
else:
    print("Not found in 10 iterations.")
```

