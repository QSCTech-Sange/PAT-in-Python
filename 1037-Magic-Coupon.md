---
title: <Python><PAT> 1037 Magic Coupon
date: 2019-08-25 09:21:43
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 大水题
---

# 题目

he magic shop in Mars is offering some magic coupons. Each coupon has an integer *N* printed on it, meaning that when you use this coupon with a product, you may get *N* times the value of that product back! What is more, the shop also offers some bonus product for free. However, if you apply a coupon with a positive *N* to this bonus product, you will have to pay the shop *N* times the value of the bonus product... but hey, magically, they have some coupons with negative *N*'s!

For example, given a set of coupons { 1 2 4 −1 }, and a set of product values { 7 6 −2 −3 } (in Mars dollars M\$) where a negative value corresponds to a bonus product. You can apply coupon 3 (with *N* being 4) to product 1 (with value M\$7) to get M​\$28 back; coupon 2 to product 2 to get M\$12 back; and coupon 4 to product 4 to get M$3 back. On the other hand, if you apply coupon 3 to product 4, you will have to pay M​\$12 to the shop.

Each coupon and each product may be selected at most once. Your task is to get as much money back as possible.

### Input Specification:

Each input file contains one test case. For each case, the first line contains the number of coupons $N_C$, followed by a line with $N_C$ coupon integers. Then the next line contains the number of products , $N_P$followed by a line with $N_P$ product values. Here $1≤N_C,N_P≤10^5$, and it is guaranteed that all the numbers will not exceed $2^{30}$.

### Output Specification:

For each test case, simply print in a line the maximum amount of money you can get back.

### Sample Input:

```in
4
1 2 4 -1
4
7 6 -2 -3
```

### Sample Output:

```out
43
```

# 题解

## 思路

+ 题目说了一大堆，其实题目无非就是
  + 给你两个序列
  + 从第一个序列挑数乘以第二序列挑一个数
  + 使这些乘积最大
  + 数选了一次就不能选，但是可以一次都不选
+ 因为正的越大乘以正的越大，效果越好，负的越大乘以负的越大效果越好，而一正一负就不乘了。

+ 那我们把两个序列按照正负分为四个序列，一正一负二正二负，同时排序
+ 利用zip和sum，对两个正序列的值相乘，对两个负序列的值相乘，并加起来

## 数据结构

+ a 是第一个序列
+ a_pos 是第一个序列的正数
+ a_neg 是第一个序列的负数
+ b 是第一个序列
+ b_pos 是第一个序列的正数
+ b_neg 是第一个序列的负数

## 算法

+ 对a_pos,b_pos按从大到小排序
+ 对a_neg,b_neg按从小到大排序
+ 将a_pos,b_pos用zip组合起来，对组合起来的每一项，求它们的乘积，并把所有乘积加起来。
+ 对a_neg,b_neg同理

## 代码

+ 因为使用Python就能AC，因此只使用了Python的代码。

```python
num_a = int(input())
a = list(map(int, input().split()))
a_pos = sorted([i for i in a if i > 0], reverse=True)
a_neg = sorted([i for i in a if i < 0])
num_b = int(input())
b = list(map(int, input().split()))
b_pos = sorted([i for i in b if i > 0], reverse=True)
b_neg = sorted([i for i in b if i < 0])
print(sum([i * j for i, j in zip(a_pos, b_pos)]) + sum([i * j for i, j in zip(a_neg, b_neg)]))
```

