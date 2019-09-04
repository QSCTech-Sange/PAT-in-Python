---
title: <Python><PAT> 1122 Hamiltonian Cycle
date: 2019-09-04 22:20:53
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 按图索骥怼就完事儿了
---

# 题目

The "Hamilton cycle problem" is to find a simple cycle that contains every vertex in a graph. Such a cycle is called a "Hamiltonian cycle".

In this problem, you are supposed to tell if a given cycle is a Hamiltonian cycle.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive integers *N* (2<*N*≤200), the number of vertices, and *M*, the number of edges in an undirected graph. Then *M* lines follow, each describes an edge in the format `Vertex1 Vertex2`, where the vertices are numbered from 1 to *N*. The next line gives a positive integer *K* which is the number of queries, followed by *K* lines of queries, each in the format:

*n* *V*1 *V*2 ... *V**n*

where *n* is the number of vertices in the list, and *V**i*'s are the vertices on a path.

### Output Specification:

For each query, print in a line `YES` if the path does form a Hamiltonian cycle, or `NO` if not.

### Sample Input:

```in
6 10
6 2
3 4
1 5
2 5
3 1
4 1
1 6
6 3
1 2
4 5
6
7 5 1 4 3 6 2 5
6 5 1 4 3 6 2
9 6 2 1 6 3 4 5 2 6
4 1 2 5 1
7 6 1 3 4 5 2 6
7 6 1 2 5 4 3 1
```

### Sample Output:

```out
YES
NO
NO
NO
YES
NO
```

# 题解

## 思路

+ 题目的意思是
+ 给你一张图
+ 给你一条路径
+ 让你判断这个路径能不能把所有的节点走完，最后形成一个环。

## 数据结构

+ edges 是一个列表，存放所有的路径
  + 下标是节点的id
  + 值是一个集合，存放这个节点的所有邻居
+ path是给出的查询路径

## 算法

+ 读入道路数据
+ 读入查询路径
+ 进入judge()函数判断
+ 如果路径的长度不等于节点数量+1 ，或者起始和结尾不是同一个节点，或者路径里面不包含所有的节点，直接返回no
+ 否则，看每个节点的下一个节点是不是在上一个节点的路径当中
+ 完事儿，简单嗷

## 代码

+ 由于使用Python可以AC，因此只放了Python的题解。

```python
def judge(path):
    global num_vertice
    if len(path) != num_vertice + 1 or path[0] != path[-1] or set(path) != set([i for i in range(1, num_vertice + 1)]):
        return "NO"
    else:
        for i in range(num_vertice):
            node = path[i]
            if path[i + 1] not in edges[node]:
                return "NO"
        return "YES"


num_vertice, num_edge = list(map(int, input().split()))
edges = [set() for _ in range(num_vertice + 1)]
for _ in range(num_edge):
    a, b = list(map(int, input().split()))
    edges[a].add(b)
    edges[b].add(a)
num_que = int(input())
for _ in range(num_que):
    path = list(map(int, input().split()[1:]))
    print(judge(path))

```

