---
title: <Python><PAT> 1142 Maximal Clique
date: 2019-09-06 13:18:59
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 图与节点
---

# 题目

A **clique** is a subset of vertices of an undirected graph such that every two distinct vertices in the clique are adjacent. A **maximal clique** is a clique that cannot be extended by including one more adjacent vertex. (Quoted from https://en.wikipedia.org/wiki/Clique_(graph_theory))

Now it is your job to judge if a given subset of vertices can form a maximal clique.

### Input Specification:

Each input file contains one test case. For each case, the first line gives two positive integers Nv (≤ 200), the number of vertices in the graph, and Ne, the number of undirected edges. Then Ne lines follow, each gives a pair of vertices of an edge. The vertices are numbered from 1 to Nv.

After the graph, there is another positive integer M (≤ 100). Then M lines of query follow, each first gives a positive number K (≤ Nv), then followed by a sequence of K distinct vertices. All the numbers in a line are separated by a space.

### Output Specification:

For each of the M queries, print in a line `Yes` if the given subset of vertices can form a maximal clique; or if it is a clique but not a **maximal clique**, print `Not Maximal`; or if it is not a clique at all, print `Not a Clique`.

### Sample Input:

```in
8 10
5 6
7 8
6 4
3 6
4 5
2 3
8 2
2 7
5 3
3 4
6
4 5 4 3 6
3 2 8 7
2 2 3
1 1
3 4 3 6
3 3 2 1
```

### Sample Output:

```out
Yes
Yes
Yes
Yes
Not Maximal
Not a Clique
```

# 题解

## 思路

+ 题目的意思是
+ 有些节点集合，它们是两两互相连接的
+ 给定你一些查询（集合），让你判断是不是两两互相连接的，以及是不是最大的这种集合。

## 数据结构

+ neighbor 记录所有节点的邻居节点
  + 下标是节点的id
  + 值是这个节点的所有邻居们，是一个集合
+ left_nodes 是这次查询中，不在查询的节点集合中的其他节点

## 算法

+ 读取数据，添加到neighbor中
+ 注意自己也是自己的neighbor
+ 读取查询，调用judge()来进行判断。
  + 对查询中的每一个节点
    + 对于其他所有的节点
      + 如果它不在这个节点的邻居中
      + 说明不是这种集合
  + 如果都符合了，那么看它是不是最大的。
  + 建立在查询节点之外的剩余节点集合
  + 遍历剩余节点
    + 遍历查询节点的每一个节点
      + 如果这个剩余节点不在这个节点的邻居中，
      + 直接break，去查询剩余节点的下一个节点
    + 如果出现了一个剩余节点是所有查询节点的邻居，那么说明这个集合不是最小的。
  + 所有的剩余节点都不是查询节点的邻居，回答yes

## 代码

+ 由于使用Python可以AC，因此只放了Python的题解。

```python
def judge(nodes):
    global num_vertice
    for node in nodes:
        for other in nodes:
            if other not in neighbor[node]:
                return "Not a Clique"
    left_nodes = [i for i in range(1,num_vertice+1) if i not in nodes]
    for node in left_nodes:
        for i in nodes:
            if node not in neighbor[i]:
                break
        else:
            return "Not Maximal"
    return "Yes"


num_vertice, num_roads = list(map(int, input().split()))
neighbor = [{i} for i in range(num_vertice + 1)]
for _ in range(num_roads):
    a, b = list(map(int, input().split()))
    neighbor[a].add(b)
    neighbor[b].add(a)

for _ in range(int(input())):
    nodes = list(map(int,input().split()[1:]))
    print(judge(set(nodes)))
```

