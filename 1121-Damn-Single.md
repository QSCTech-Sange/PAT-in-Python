---
title: <Python><PAT> 1121 Damn Single
date: 2019-09-04 21:10:55
tags:
- PAT
- 题解
- 哈希表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 当你的对象不在party里，你就算单身了？？？
---

# 题目

"Damn Single (单身狗)" is the Chinese nickname for someone who is being single. You are supposed to find those who are alone in a big party, so they can be taken care of.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤ 50,000), the total number of couples. Then N lines of the couples follow, each gives a couple of ID's which are 5-digit numbers (i.e. from 00000 to 99999). After the list of couples, there is a positive integer M (≤ 10,000) followed by M ID's of the party guests. The numbers are separated by spaces. It is guaranteed that nobody is having bigamous marriage (重婚) or dangling with more than one companion.

### Output Specification:

First print in a line the total number of lonely guests. Then in the next line, print their ID's in increasing order. The numbers must be separated by exactly 1 space, and there must be no extra space at the end of the line.

### Sample Input:

```in
3
11111 22222
33333 44444
55555 66666
7
55555 44444 10000 88888 22222 11111 23333
```

### Sample Output:

```out
5
10000 23333 44444 55555 88888
```

# 题解

## 思路

+ 这题真实迷惑
+ 题目给出互为夫妻的两个人
+ 给出参与派对的人
+ 问你哪些人是单身的
+ 迷惑的是，当一个人是已婚的，他对象没出现在party中，也当作单身处理

## 数据结构

+ couple为一个哈希表，键和值互为一对couple
+ ans 存放答案
+ guests 是一个集合，存所有的宾客。（为了方便查找设置成了哈希集）

## 算法

+ 将成对成对的情侣添加到couple里
+ 遍历所有宾客
  + 如果宾客不在couple里或者他的对象不在couple里
    + 把它添加到ans里
+ 对ans排序并输出

## 代码

+ 由于使用Python能够AC，因此使用Python作题解。

```python
n = int(input())
ans = []
couple = dict()
for _ in range(n):
    a, b = input().split()
    couple[a] = b
    couple[b] = a

n = int(input())
guests = set(input().split())

for i in guests:
    if i not in couple or couple[i] not in guests:
        ans.append(i)

print(len(ans))
if len(ans)!= 0:
    print(" ".join(sorted(ans)))

```

