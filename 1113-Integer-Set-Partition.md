---
title: <Python><PAT> 1113 Integer Set Partition
date: 2019-09-04 12:43:19
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 超级憨憨题，占了Python整数精度无限制的便宜
---

# 题目

Given a set of *N* (>1) positive integers, you are supposed to partition them into two disjoint sets *A*1 and *A*2 of *n*1 and *n*2 numbers, respectively. Let *S*1 and *S*2 denote the sums of all the numbers in *A*1 and *A*2, respectively. You are supposed to make the partition so that ∣*n*1−*n*2∣ is minimized first, and then ∣*S*1−*S*2∣ is maximized.

### Input Specification:

Each input file contains one test case. For each case, the first line gives an integer $N (2≤N≤10^5)$, and then *N* positive integers follow in the next line, separated by spaces. It is guaranteed that all the integers and their sum are less than $2^{31}$.

### Output Specification:

For each case, print in a line two numbers: ∣*n*1−*n*2∣ and ∣*S*1−*S*2∣, separated by exactly one space.

### Sample Input 1:

```in
10
23 8 10 99 46 2333 46 1 666 555
```

### Sample Output 1:

```out
0 3611
```

### Sample Input 2:

```in
13
110 79 218 69 3721 100 29 135 2 6 13 5188 85
```

### Sample Output 2:

```out
1 9359
```

# 题解

## 思路

+ 憨憨题
+ 给你一个数列，让你尽可能等半分成两类，然后尽可能两类的差最大
+ 那么我们排序，大的一半分一类，少的分一类，分不均的归大的。
+ 输出即可

## 数据结构

+ nums 原数序列

## 算法

+ 排序
+ 做差
+ 输出

## 代码

+ 由于使用Python3能AC，因此只放了Python的题解。

```python
n = int(input())
nums = sorted(list(map(int, input().split())), reverse=True)
print(n % 2, sum(nums[:(n + 1) // 2]) - sum(nums[((n + 1) // 2):]))
```

