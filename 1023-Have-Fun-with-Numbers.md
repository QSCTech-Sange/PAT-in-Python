---
title: <Python><PAT> 1023 Have Fun with Numbers
date: 2019-08-22 19:33:36
tags: 
- PAT
- 题解
- 哈希表
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 字符串 + 哈希表，Python Python你最屌
---

# 题目

Notice that the number 123456789 is a 9-digit number consisting exactly the numbers from 1 to 9, with no duplication. Double it we will obtain 246913578, which happens to be another 9-digit number consisting exactly the numbers from 1 to 9, only in a different permutation. Check to see the result if we double it again!

Now you are suppose to check if there are more numbers with this property. That is, double a given number with *k* digits, you are to tell if the resulting number consists of only a permutation of the digits in the original number.

### Input Specification:

Each input contains one test case. Each case contains one positive integer with no more than 20 digits.

### Output Specification:

For each test case, first print in a line "Yes" if doubling the input number gives a number that consists of only a permutation of the digits in the original number, or "No" if not. Then in the next line, print the doubled number.

### Sample Input:

```in
1234567899
```

### Sample Output:

```out
Yes
2469135798
```

# 题解

## 思路

+ 建立一个哈希表，存放每个字符出现的次数
+ 将原数乘以2后，和哈希表每个字符出现的次数作对比
+ 完事儿嗷

## 数据结构

+ _dict 是一个哈希表，对每个键默认值是0
  + 键是0到9的字符
  + 值是它出现的次数

## 算法

+ 对作为字符串的原数，按照每一个字符出现的次数添加到哈希表
+ 将原数乘以2,对每一个出现的字符，哈希表相应减一
+ 如果哈希表不是每一项都为0,那么就说明是No

## 代码

因为使用Python能AC，因此只放了Python的代码。

```python
from collections import defaultdict

_dict = defaultdict(int)
a = input()
for i in a:
    _dict[i] += 1
a = str(int(a) * 2)
for i in a:
    _dict[i] -= 1
for i in "0123456789":
    if _dict[i] != 0:
        print("No")
        break
else:
    print("Yes")
print(a)

```

