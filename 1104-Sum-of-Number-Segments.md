---
title: <Python><PAT> 1104 Sum of Number Segments
date: 2019-09-03 14:38:42
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 找呀找呀找规律
---

# 题目

Given a sequence of positive numbers, a segment is defined to be a consecutive subsequence. For example, given the sequence { 0.1, 0.2, 0.3, 0.4 }, we have 10 segments: (0.1) (0.1, 0.2) (0.1, 0.2, 0.3) (0.1, 0.2, 0.3, 0.4) (0.2) (0.2, 0.3) (0.2, 0.3, 0.4) (0.3) (0.3, 0.4) and (0.4).

Now given a sequence, you are supposed to find the sum of all the numbers in all the segments. For the previous example, the sum of all the 10 segments is 0.1 + 0.3 + 0.6 + 1.0 + 0.2 + 0.5 + 0.9 + 0.3 + 0.7 + 0.4 = 5.0.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N*, the size of the sequence which is no more than 105. The next line contains *N* positive numbers in the sequence, each no more than 1.0, separated by a space.

### Output Specification:

For each test case, print in one line the sum of all the numbers in all the segments, accurate up to 2 decimal places.

### Sample Input:

```in
4
0.1 0.2 0.3 0.4
```

### Sample Output:

```out
5.00
```

# 题解

## 思路

+ 给出一个序列，求所有子序列的所有值的和
+ 核心是求一个数出现了几次
+ 我们不妨看例子中给的序列
  + (0.1) (0.1, 0.2) (0.1, 0.2, 0.3) (0.1, 0.2, 0.3, 0.4) 
  + (0.2) (0.2, 0.3) (0.2, 0.3, 0.4) 
  + (0.3) (0.3, 0.4)
  + (0.4).
+ 我按照以不同数作为序列起始来划分。
+ 先看1,在上面是不可能存在来，在右边有4个。
+ 再看2,上面出现了3次，右边出现了3次
+ 再看3,上面出现了2 +  2 次，右边出现了2次
+ 再看4,右边出现了1次，上面出现了1 + 1 + 1 即3次
+ 所以一个数出现了多少次，我们可以从第0层到这一层来划分，每一层出现的次数一定是n-i，n为总数，i为下标。而层数就是(i+1)
+ 这样就迎刃而解了

## 数据结构

+ 没有。

## 算法

+ 参照思路。

## 代码

+ 由于使用Python可以AC，因此只放了Python的解。
+ 我这里只用了两行，可能理解会困难，你可以将它展开成很多行来理解。

```python
n = int(input())
print("%.2f" % sum([i * j for i, j in zip(list(map(float, input().split())), [(n - i) * (i + 1) for i in range(n)])]))

```

