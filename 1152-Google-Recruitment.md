---
title: <Python><PAT> 1152 Google Recruitment
date: 2019-09-07 11:29:13
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 找素数
---

# 题目

In July 2004, Google posted on a giant billboard along Highway 101 in Silicon Valley (shown in the picture below) for recruitment. The content is super-simple, a URL consisting of the first 10-digit prime found in consecutive digits of the natural constant *e*. The person who could find this prime number could go to the next step in Google's hiring process by visiting this website.

![prime](https://images.ptausercontent.com/57148679-d574-4f49-b048-775c6c07791c.jpg)

The natural constant *e* is a well known transcendental number（超越数）. The first several digits are: *e* = 2.71828182845904523536028747135266249775724709369995957496696762772407663035354759457138217852516642**7427466391**932003059921... where the 10 digits in bold are the answer to Google's question.

Now you are asked to solve a more general problem: find the first K-digit prime in consecutive digits of any given L-digit number.

### Input Specification:

Each input file contains one test case. Each case first gives in a line two positive integers: L (≤ 1,000) and K (< 10), which are the numbers of digits of the given number and the prime to be found, respectively. Then the L-digit number N is given in the next line.

### Output Specification:

For each test case, print in a line the first K-digit prime in consecutive digits of N. If such a number does not exist, output `404` instead. Note: the leading zeroes must also be counted as part of the K digits. For example, to find the 4-digit prime in 200236, 0023 is a solution. However the first digit 2 must not be treated as a solution 0002 since the leading zeroes are not in the original number.

### Sample Input 1:

```in
20 5
23654987725541023819
```

### Sample Output 1:

```out
49877
```

### Sample Input 2:

```in
10 3
2468024680
```

### Sample Output 2:

```out
404
```

# 题解

## 思路

+ 给定一场串数和一个长度，让你在这串数中找到一个字符串，组成的数字是素数，字符串的长度刚好等于给定的。有多个就排在第一个的。
+ 那么就以这个长度为窗口，不断滑动，即可。
+ 注意输出的时候要输出切分的字符串，不能输出整数，不然前面的0会被吃掉。

## 数据结构

+ num 是原数
+ test 是切分后的字符串

## 算法

+ 求素数的算法
  + 先判断123
  + 再判断偶数
  + 再从3到sqrt(num)，每隔2个递增，依次判断这个数能不能被遍历的数整除

## 代码

+ 由于使用Python能够AC，因此只放了Python的题解。

```python
def isPrime(a):
    if a == 1:
        return False
    if a == 2 or a == 3:
        return True
    if a % 2 == 0:
        return False
    for i in range(3, int(a ** 0.5) + 1, 2):
        if a % i == 0:
            return False
    return True


length, digit = list(map(int, input().split()))
num = input()
for i in range(len(num) - digit + 1):
    test = num[i:i + digit]
    if isPrime(int(test)):
        print(test)
        break
else:
    print("404")
```

