---
title: <Python><PAT> 1027 Colors in Mars
date: 2019-08-23 18:57:45
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 太简单了，没啥好讲的。
---

# 题目

People in Mars represent the colors in their computers in a similar way as the Earth people. That is, a color is represented by a 6-digit number, where the first 2 digits are for `Red`, the middle 2 digits for `Green`, and the last 2 digits for `Blue`. The only difference is that they use radix 13 (0-9 and A-C) instead of 16. Now given a color in three decimal numbers (each between 0 and 168), you are supposed to output their Mars RGB values.

### Input Specification:

Each input file contains one test case which occupies a line containing the three decimal color values.

### Output Specification:

For each test case you should output the Mars RGB value in the following format: first output `#`, then followed by a 6-digit number where all the English characters must be upper-cased. If a single color is only 1-digit long, you must print a `0` to its left.

### Sample Input:

```in
15 43 71
```

### Sample Output:

```out
#123456
```

# 题解

## 思路

+ 十进制到十三进制的转换

## 数据结构

+ info是一个列表，总共三个元素，保存了RGB的十进制表示。

## 算法

+ 主要就是十进制转十三进制
+ 每次将原数取13的模，加在转换数的前面
+ 原数整除13
+ 直到原数为0
+ 最后处理一下位数不够的情况

## 代码

因为使用Python3能AC，因此只放了Python的代码。

```python
def ten_to_13(a):
    ans = ""
    while a != 0:
        temp = a % 13
        if temp >= 10:
            temp = chr(temp - 10 + ord('A'))
        else:
            temp = str(temp)
        ans = temp + ans
        a //= 13
    if len(ans) == 0:
        return "00"
    if len(ans) == 1:
        return "0" + ans
    return ans


info = list(map(int, input().split()))
ans = "#"
for i in info:
    ans += ten_to_13(i)
print(ans)
```

