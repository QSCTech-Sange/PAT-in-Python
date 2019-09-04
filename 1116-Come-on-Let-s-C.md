---
title: <Python><PAT> 1116 Come on! Let's C
date: 2019-09-04 16:18:23
tags:
- PAT
- 题解
- 哈希表
- 哈希集合
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 憨憨题，就是纯粹handle多种情况
---

# 题目

"Let's C" is a popular and fun programming contest hosted by the College of Computer Science and Technology, Zhejiang University. Since the idea of the contest is for fun, the award rules are funny as the following:

- 0、 The Champion will receive a "Mystery Award" (such as a BIG collection of students' research papers...).
- 1、 Those who ranked as a prime number will receive the best award -- the Minions (小黄人)!
- 2、 Everyone else will receive chocolates.

Given the final ranklist and a sequence of contestant ID's, you are supposed to tell the corresponding awards.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer $N (≤10^4)$, the total number of contestants. Then N lines of the ranklist follow, each in order gives a contestant's ID (a 4-digit number). After the ranklist, there is a positive integer K followed by K query ID's.

### Output Specification:

For each query, print in a line `ID: award` where the award is `Mystery Award`, or `Minion`, or `Chocolate`. If the ID is not in the ranklist, print `Are you kidding?` instead. If the ID has been checked before, print `ID: Checked`.

### Sample Input:

```in
6
1111
6666
8888
1234
5555
0001
6
8888
0001
1111
2222
8888
2222
```

### Sample Output:

```out
8888: Minion
0001: Chocolate
1111: Mystery Award
2222: Are you kidding?
8888: Checked
2222: Are you kidding?
```

# 题解

## 思路

+ 憨憨题
+ 读取所有人和排名，添加到哈希表里
+ 遍历每次查询，按照是否是素数，是否检查过，是否是第一名的原则来输出
+ 查询后将其添加到checked中

## 数据结构

+ rank 是一个哈希表，将id映射到排名
+ checked 是一个哈希集，记录已经查询过的人的id

## 算法

+ 如何判断是否是素数
  + 先判断1,2,3的情况
  + 如果数能被2整除，那么不是素数
  + 步长为2遍历从3到sqrt(原数)
    + 当原数能被这个遍历着的数整除的时候，说明它不是素数
  + 都能整除说明它是素数
+ 这个算法相对复杂度小一点。

## 代码

+ 由于使用Python可以AC，因此只放了Python的题解。

```python
n = int(input())
rank = dict()
checked = set()
for i in range(n):
    rank[input()] = i + 1

n = int(input())
for _ in range(n):
    id = input()
    if id not in rank:
        print("%s: Are you kidding?" % id)
    else:
        if id not in checked:
            if rank[id] == 1:
                print("%s: Mystery Award" % id)
            elif rank[id] == 2 or rank[id] == 3:
                print("%s: Minion" % id)
            elif rank[id] % 2 == 0:
                print("%s: Chocolate" % id)
            else:
                for i in range(3, int(rank[id] ** 0.5) + 1, 2):
                    if rank[id] % i == 0:
                        print("%s: Chocolate" % id)
                        break
                else:
                    print("%s: Minion" % id)
            checked.add(id)
        else:
            print("%s: Checked" % id)
```

