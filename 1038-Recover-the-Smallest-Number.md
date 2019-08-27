---
title: <Python><PAT> 1038 Recover the Smallest Number
date: 2019-08-25 14:37:48
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 震惊！Python一行代码搞定30分大题
---

# 题目

Given a collection of number segments, you are supposed to recover the smallest number from them. For example, given { 32, 321, 3214, 0229, 87 }, we can recover many numbers such like 32-321-3214-0229-87 or 0229-32-87-321-3214 with respect to different orders of combinations of these segments, and the smallest number is 0229-321-3214-32-87.

### Input Specification:

Each input file contains one test case. Each case gives a positive integer *N* (≤104) followed by *N* number segments. Each segment contains a non-negative integer of no more than 8 digits. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the smallest number in one line. Notice that the first digit must not be zero.

### Sample Input:

```in
5 32 321 3214 0229 87
```

### Sample Output:

```out
22932132143287
```

# 题解

## 思路

+ 题目意思是让你从给定的几个数中组合起来构造成最小数
+ 核心是数字的排部
+ 一般很容易想到，字符串比较大小中，小的放前面，大的放后面
+ 但这实际上有个问题，比如题目里给的32和321
+ 合理的处理是32132，但是字符串比较的话会排成32321
+ 所以我们实际上比较的是
+ 两个字符串合起来的前后两种组合中，哪个小
+ 这才是我们的排序依据

## 数据结构

+ 就一行能有啥数据结构

## 算法

+ 读入数据，不算第一行第一项，因为它是告诉你总共有几个数的，而不是真实的数之一
+ 将其排序
  + 注意排序的依据，这点写在思路里了
  + 因为python3的排序指定key不能对两项来指定，所以我们需要导入`cmp_to_key`来帮忙
    + 注意这里的排序函数要返回大于0还是小于0,不是返回True或False
    + 所以我们得先转成整数，然后相减即可
+ 排序完成后，用join将列表里所有项连接起来
+ 去除左边的0
+ 如果答案是空的话，要返回0而不能返回空
+ 在Python3里，`c = a or b` 的意思就是，如果a是False的，那么c = b。在这里面，就是如果前面是空的那么返回0。同理`c = a and b`的意思是如果a 是True的，那么c = b

##　代码

+ 使用Python3一行代码就能AC，因此只放了Python3的代码。
+ 引入库没算在一行内
+ 如果使用Python2的话，可以不用引入库，因为Python2里的sorted()有cmp这个参数，可以在cmp里用lambda来指定排列依据。

```python
from functools import cmp_to_key
print("".join(sorted(input().split()[1:], key=cmp_to_key(lambda x,y:int(x + y) - int(y + x)))).lstrip("0") or "0")

```

