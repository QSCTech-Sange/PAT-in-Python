---
title: <Python><PAT> 1063 Set Similarity
date: 2019-08-29 09:48:09
tags: 
- PAT
- 题解
- 哈希集
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 看到集合用Python啊，直接爽到飞起来。
---

# 题目

Given two sets of integers, the similarity of the sets is defined to be $N_{c} / N_{t} \times 100 \%,$ where $N_{c}$ is the number of distinct common numbers shared by the two sets, and $N_{t}$ is the total number of distinct numbers in the two sets. Your job is to calculate the similarity of any given pair of sets.

### Input Specification:

Each input file contains one test case. Each case first gives a positive integer$N( \leq 50)$which is the total number of sets. Then *N* lines follow, each gives a set with a positive $M(\leq 10 ^ 4)$ and followed by *M* integers in the range $[0,10^9]$. After the input of sets, a positive integer *K* (≤2000) is given, followed by *K* lines of queries. Each query gives a pair of set numbers (the sets are numbered from 1 to *N*). All the numbers in a line are separated by a space.

### Output Specification:

For each query, print in one line the similarity of the sets, in the percentage form accurate up to 1 decimal place.

### Sample Input:

```in
3
3 99 87 101
4 87 101 5 87
7 99 101 18 5 135 18 99
2
1 2
1 3
```

### Sample Output:

```out
50.0%
33.3%
```

# 题解

## 思路

+ Python的集合有交，并这些自带的函数
+ So，你懂得。

## 数据结构

+ sets 存放所有的集合
+ a,b是要计算的两个集合

## 算法

+ 使用自带的计算集合交并的方式。

## 代码

+ 由于使用Python能AC，因此只放了Python的代码。

```python
sets = []
for _ in range(int(input())):
    sets.append(set(map(int, input().split()[1:])))
for _ in range(int(input())):
    a, b = list(map(lambda x: int(x) - 1, input().split()))
    print("%.1f%%" % ((len(sets[a] & sets[b]) / len(sets[a] | sets[b]))*100))

```