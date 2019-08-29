---
title: <Python><PAT> 1049 Counting Ones
date: 2019-08-27 11:10:27
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 请零注意，来找一啦！
---

# 题目

The task is simple: given any positive integer *N*, you are supposed to count the total number of 1's in the decimal form of the integers from 1 to *N*. For example, given *N* being 12, there are five 1's in 1, 10, 11, and 12.

### Input Specification:

Each input file contains one test case which gives the positive $N(\leq 2 ^ {30})$.

### Output Specification:

For each test case, print the number of 1's in one line.

### Sample Input:

```in
12
```

### Sample Output:

```out
5
```

# 题解

## 思路

+ 题目的意思是
+ 给你一个数
+ 让你从0到这个数中间的所有数中，找到出现1的个数。
+ 不妨开始找规律
+ 先从个位数看
  + 如果个位数是1,那么加1
  + 如果个位数大于1,那么加1
+ 再看十位数`j`
  + 如果十位数是1,那么加上个位数再加上2
  + 如果十位数是大于1,那么加上这个`10*j  + 10`
+ 再看百位数`j`
  + 如果百位数是1,那么加上十位数百位数组成的数字再加上21
  + 如果百位数是大于1,那么加上这个`100 * j  + 100`
+ 再看千位数`j`
  - 如果千位数是1,那么加上百位数及后面几位所组成的数字再加上301
  - 如果千位数是大于1,那么加上这个`1000 * j  + 1000`
+ 这下就很容易发现规律了。
+ 我们所求的值只需要遍历所有位数，并且加总，即可。

## 数据结构

+ num是原数

## 算法

+ 将原数逆序以从个位数开始计算
+ 按照思路里的方法做就行了。
+ 注意当一个字符串为空的时候，取int会报错，这里要做一个额外的判断。

## 代码

+ 由于使用Python能够AC，因此只放了Python的代码。

```python
num = input()
num = num[::-1]
sum = 0
for i in range(len(num)):
    if int(num[i]) == 1:
        sum += (int(num[:i][::-1]) + int(i * (10 ** (i - 1)) + 1)) if len(num[:i]) > 0 else 1
    elif int(num[i]) > 1:
        sum += int(int(num[i]) * i * (10 ** (i - 1)) + 10 ** i)
print(sum)
```

