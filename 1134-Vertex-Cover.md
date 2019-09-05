---
title: <Python><PAT> 1134 Vertex Cover
date: 2019-09-05 19:43:39
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 核心是道路如何表示？
---

# 题目

A **vertex cover** of a graph is a set of vertices such that each edge of the graph is incident to at least one vertex of the set. Now given a graph with several vertex sets, you are supposed to tell if each of them is a vertex cover or not.

### Input Specification:

Each input file contains one test case. For each case, the first line gives two positive integers *N* and *M* (both no more than 104), being the total numbers of vertices and the edges, respectively. Then *M* lines follow, each describes an edge by giving the indices (from 0 to *N*−1) of the two ends of the edge.

After the graph, a positive integer *K* (≤ 100) is given, which is the number of queries. Then *K* lines of queries follow, each in the format:
$$
N_v\; v[1]\; v[2] \cdots v[N_v]
$$
where $N_v$ is the number of vertices in the set, and *v*[*i*]'s are the indices of the vertices.

### Output Specification:

For each query, print in a line `Yes` if the set is a vertex cover, or `No` if not.

### Sample Input:

```in
10 11
8 7
6 8
4 5
8 4
8 1
1 2
1 4
9 8
9 1
1 0
2 4
5
4 0 3 8 4
6 6 1 7 5 4 9
3 1 8 4
2 2 8
7 9 8 7 6 5 4 2
```

### Sample Output:

```out
No
Yes
Yes
No
No
```

# 题解

## 思路

+ 题目意思是给你一个图，然后给你一堆节点，让你判断和这些点相邻的所有道路是不是涵盖了整张图的道路
+ 所以我们要考虑的首要问题是怎么表示一个道路
+ 我们可以给定一个虚拟的道路编号，从0开始，一直到给定的道路数量-1
+ 这样子，我们可以在读取的时候，读到两个节点，那么这两个节点的道路数组中都添加这一条道路编号，同时道路编号+1
+ 在查询的时候，我们预设每条道路都是没有访问过的。
+ 然后对查询的每个节点，标记它的所有道路为访问过
+ 最后遍历所有的道路有没有被访问过即可。

## 数据结构

+ roads 是道路数组，包含了所有的道路
  + 下标是节点的id
  + 值是这个节点所联通的所有道路的编号的数组
+ index 是边读取边更新的道路下标
+ nodes 是所有查询中读取到的节点列表
+ visited 标记哪些道路是已经走过的列表
  + 下标是道路id
  + 值是True或False

## 算法

+ 都写在思路里了。

## 代码

+ 由于使用Python能AC，因此使用了Python的题解。

```python
num_vertice, num_edge = list(map(int, input().split()))
roads = [[] for _ in range(num_vertice)]
# 读入
index = 0
for _ in range(num_edge):
    a,b = list(map(int, input().split()))
    roads[a].append(index)
    roads[b].append(index)
    index += 1
# 查询
for _ in range(int(input())):
    nodes = list(map(int, input().split()[1:]))
    visited = [False for _ in range(num_edge)]
    for node in nodes:
        for road in roads[node]:
            visited[road] = True
    if all(visited):
        print("Yes")
    else:
        print("No")
```

