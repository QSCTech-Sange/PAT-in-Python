---
title: <Python><PAT> 1094 The Largest Generation
date: 2019-08-16 18:34:35
tags: 
- PAT
- 题解
- BFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 非常基础的BFS遍历，难度简单。
---

# 题目

A family hierarchy is usually presented by a pedigree tree where all the nodes on the same level belong to the same generation. Your task is to find the generation with the largest population.

### Input Specification:

Each input file contains one test case. Each case starts with two positive integers *N* (<100) which is the total number of family members in the tree (and hence assume that all the members are numbered from 01 to *N*), and *M* (<*N*) which is the number of family members who have children. Then *M* lines follow, each contains the information of a family member in the following format:

```
ID K ID[1] ID[2] ... ID[K]
```

where `ID` is a two-digit number representing a family member, `K` (>0) is the number of his/her children, followed by a sequence of two-digit `ID`'s of his/her children. For the sake of simplicity, let us fix the root `ID` to be `01`. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in one line the largest population number and the level of the corresponding generation. It is assumed that such a generation is unique, and the root level is defined to be 1.

### Sample Input:

```in
23 13
21 1 23
01 4 03 02 04 05
03 3 06 07 08
06 2 12 13
13 1 21
08 2 15 16
02 2 09 10
11 2 19 20
17 1 22
05 1 11
07 1 14
09 1 17
10 1 18
```

### Sample Output:

```out
9 4
```

# 题解

### 思路

+ 最简单的BFS遍历了，甚至不用记录有没有被访问过。
+ 从根节点开始BFS，建立一个队列，每次从队列取出一个节点，遍历它的所有孩子，使得它所有孩子的层级等于这个节点 +1，同时把这个节点放入队列当中
+ 在我们遍历的同时，可以同时计算最大的孩子数量，因为我们是广度优先，换句话说就是层次优先，从低层次到高层次，低层次遍历完了才遍历高层次，所以可以计算出每个层次的人数。

### 代码

这题使用Python能AC，所以就只放了Python的代码。

#### 数据结构

+ level 是一个列表，下标是id，值代表这个id的层数，一开始都是1
+ sons 是一个列表，下标是id，值代表这个id的所有孩子，也是一个列表。
+ que 是一个队列，帮助我们BFS
+ max的level和num记录目前已有的最大的num以及对应的level
+ temp的level和num记录目前正在计算过程中的level和num

#### 算法

+ 读入数据并初始化数据结构
+ 建立一个队列，把根节点放进去
+ 当队列为空的时候，说明遍历结束
+ 每次从队列里取一个点，更新它的所有孩子的层级为这个点+1
+ 当我们发现孩子的层级大于temp_level的时候，说明这个层级已经遍历完了，那么就可以判断这层的num和max的num谁大谁小，并更新。
+ 否则，增加temp的num
+ 把这些孩子都加到队列当中
+ **注意**，这样子没有将最后一层加入到与max_sum的比较，因此最后还需要多一次对比。我一开始就因为这个错了一个点。

```Python
N, M = list(map(int, input().split()))

level = [1 for _ in range(N + 1)]
sons = [[] for _ in range(N + 1)]

for i in range(M):
    a = input().split()
    sons[int(a[0])] = list(map(int, a[2:]))

max_level, max_num, temp_level, temp_num = [1, 1, 1, 1]

que = [1]
while que:
    father = que.pop()
    for son in sons[father]:
        level[son] = level[father] + 1
        if level[son] > temp_level:
            if temp_num > max_num:
                max_num,max_level = temp_num, temp_level
            temp_level,temp_num = level[son], 1
        else:temp_num += 1
        que.insert(0, son)

if temp_num > max_num:print(temp_num, temp_level)
else:print(max_num, max_level)
```

