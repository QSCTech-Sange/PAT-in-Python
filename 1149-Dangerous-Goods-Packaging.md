---
title: <Python><PAT> 1149 Dangerous Goods Packaging
date: 2019-09-06 21:39:47
tags:
- PAT
- 题解
- 哈希集合
- 哈希表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 哈希表与哈希集的应用
---

# 题目

When shipping goods with containers, we have to be careful not to pack some incompatible goods into the same container, or we might get ourselves in serious trouble. For example, oxidizing agent （氧化剂） must not be packed with flammable liquid （易燃液体）, or it can cause explosion.

Now you are given a long list of incompatible goods, and several lists of goods to be shipped. You are supposed to tell if all the goods in a list can be packed into the same container.

### Input Specification:

Each input file contains one test case. For each case, the first line gives two positive integers: $N (≤10^4)$, the number of pairs of incompatible goods, and *M* (≤100), the number of lists of goods to be shipped.

Then two blocks follow. The first block contains N pairs of incompatible goods, each pair occupies a line; and the second one contains M lists of goods to be shipped, each list occupies a line in the following format:

```
K G[1] G[2] ... G[K]
```

where `K` (≤1,000) is the number of goods and `G[i]`'s are the IDs of the goods. To make it simple, each good is represented by a 5-digit ID number. All the numbers in a line are separated by spaces.

### Output Specification:

For each shipping list, print in a line `Yes` if there are no incompatible goods in the list, or `No` if not.

### Sample Input:

```in
6 3
20001 20002
20003 20004
20005 20006
20003 20001
20005 20004
20004 20006
4 00001 20004 00002 20003
5 98823 20002 20003 20006 10010
3 12345 67890 23333
```

### Sample Output:

```out
No
Yes
Yes
```

# 题解

## 思路

+ 我们需要找到给定的几个货物中，找出它们是否不互斥
+ 为了提高效率，当然是使用哈希表啦！

## 数据结构

+ 使用danger 作为哈希表存储危险品
  + 键是一个商品
  + 值是一个列表，存放所有与这个危险品互为危险品的商品
+ 由于都是整数，所以就用数组表示了

## 算法

+ 读取所有危险品组合
+ 两个危险品都要把互相放到自己的危险品组合里
+ 遍历查询的货物
  + 遍历这个货物对应的危险品
  + 如果它在货物里
  + 那就说明不能放在一起

## 代码

+ 由于使用Python能够AC，因此只放了Python的题解。

```python
num_pair, num_que = list(map(int, input().split()))
danger = [set() for i in range(100001)]
for _ in range(num_pair):
    a, b = list(map(int, input().split()))
    danger[a].add(b)
    danger[b].add(a)


def judge(boat):
    for thing in boat:
        for enemy in danger[thing]:
            if enemy in boat:
                return "No"
    else:
        return "Yes"


for _ in range(num_que):
    boat = set(list(map(int, input().split()[1:])))
    print(judge(boat))

```

