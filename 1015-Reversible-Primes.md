---
title: <Python><PAT> 1015 Reversible Primes
date: 2019-08-21 14:13:11
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 题目意思太过迷惑
---

# 题目

A **reversible prime** in any number system is a prime whose "reverse" in that number system is also a prime. For example in the decimal system 73 is a reversible prime because its reverse 37 is also a prime.

Now given any two positive integers *N* (<105) and *D* (1<*D*≤10), you are supposed to tell if *N* is a reversible prime with radix *D*.

### Input Specification:

The input file consists of several test cases. Each case occupies a line which contains two integers *N* and *D*. The input is finished by a negative *N*.

### Output Specification:

For each test case, print in one line `Yes` if *N* is a reversible prime with radix *D*, or `No` if not.

### Sample Input:

```in
73 10
23 2
23 10
-2
```

### Sample Output:

```out
Yes
Yes
No
```

# 思路

+ 首先理解题意，题意挺绕
+ 意思是，给你一个数和一个进制，如果这个数是质数，同时这个将这个数转换成给定的进制，再逆序，再返回十进制，还是一个质数，那么打印Yes，否则打印No
+ 所以我们需要一个判断质数的函数，一个从十进制转为任意进制的函数，一个从任意进制转换回十进制的函数
+ 其中，从任意进制转换回十进制可以用Python的`int(str,base =n)`函数实现任意n进制的字符串转为十进制的数。

## 数据结构

+ a是读进来的数的列表
+ a[0]就是数，a[1]就是进制

## 算法

+ convert()将十进制转为任意进制字符串
  + 将结果初始化为空字符串
  + 当数字不为0的时候
    + 字符串在前部加上数字模进制
    + 数字整除进制
  + 返回结果字符串
+ isPrime()判断是否是质数
  + 首先判断1和2的特判
  + 然后从3到根号原数的范围内，每隔两个数进行判断，如果原数能被这个数整除，那么就不是素数

> 更精密的判断质数的方法略去不表，记住这个一般情况下已经足够。

## 代码

因为使用Python能AC，因此只放了Python的代码。

```python
# 将十进制转为任意进制字符串
def convert(n, radix):
    ans = ""
    while n != 0:
        ans = str(n % radix) + ans
        n = n // radix
    return ans

# 判断是否是质数
def isPrime(n):
    if n == 1:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    for i in range(3, int(n ** 0.5) + 1, 2):
        if n % i == 0:
            return False
    return True

# 输入与判断
a = input().split()
while a[0][0] != '-':
    if isPrime(int(a[0])) and isPrime(int(convert(int(a[0]), int(a[1]))[::-1],int(a[1]))):
        print("Yes")
    else:
        print("No")
    a = input().split()

```

