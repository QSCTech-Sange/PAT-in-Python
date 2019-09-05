---
title: <Python><PAT> 1126 Eulerian Path
date: 2019-09-05 10:32:46
tags:
- PAT
- 题解
- DFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 欧拉回路，看似难但是题目里把条件给你了。
---

# 题目

In graph theory, an Eulerian path is a path in a graph which visits every edge exactly once. Similarly, an Eulerian circuit is an Eulerian path which starts and ends on the same vertex. They were first discussed by Leonhard Euler while solving the famous Seven Bridges of Konigsberg problem in 1736. It has been proven that connected graphs with all vertices of even degree have an Eulerian circuit, and such graphs are called **Eulerian**. If there are exactly two vertices of odd degree, all Eulerian paths start at one of them and end at the other. A graph that has an Eulerian path but not an Eulerian circuit is called **semi-Eulerian**. (Cited from https://en.wikipedia.org/wiki/Eulerian_path)

Given an undirected graph, you are supposed to tell if it is Eulerian, semi-Eulerian, or non-Eulerian.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 2 numbers N (≤ 500), and M, which are the total number of vertices, and the number of edges, respectively. Then M lines follow, each describes an edge by giving the two ends of the edge (the vertices are numbered from 1 to N).

### Output Specification:

For each test case, first print in a line the degrees of the vertices in ascending order of their indices. Then in the next line print your conclusion about the graph -- either `Eulerian`, `Semi-Eulerian`, or `Non-Eulerian`. Note that all the numbers in the first line must be separated by exactly 1 space, and there must be no extra space at the beginning or the end of the line.

### Sample Input 1:

```in
7 12
5 7
1 2
1 3
2 3
2 4
3 4
5 2
7 6
6 3
4 5
6 4
5 6
```

### Sample Output 1:

```out
2 4 4 4 4 4 2
Eulerian
```

### Sample Input 2:

```in
6 10
1 2
1 3
2 3
2 4
3 4
5 2
6 3
4 5
6 4
5 6
```

### Sample Output 2:

```out
2 4 4 4 3 3
Semi-Eulerian
```

### Sample Input 3:

```in
5 8
1 2
2 5
5 4
4 1
1 3
3 2
3 4
5 3
```

### Sample Output 3:

```out
3 3 4 3 3
Non-Eulerian
```

# 题解

##　思路

+ 欧拉图看似很难，但是实际上判断条件都给你了
+ degree是度，即一个节点的邻居数量。
+ 当一个图是连通图的时候
  + 如果节点的度都是偶数
    + 欧拉图
  + 如果节点正好有两个点的度不是偶数
    + 半欧拉图
  + 不是欧拉图
+ 否则，不是欧拉图
+ 照着这个条件理就可以了

## 数据结构

+ edges 记录所有的道路，是一个列表
  + 下标是节点id
  + 值是一个列表，存储这个节点的所有邻居
+ degree 是一个列表，记录所有的度
  + 下标是id
  + 值是度
+ odd 统计奇数的个数
+ count 计算从节点1开始的连通分量的个数
+ visited 是dfs中记录节点有没有被访问过

## 算法

+ 道路数据的录入
+ 计算度
+ 计算奇节点的个数
+ 对于奇节点的个数为0和2的情况，要看看是不是连通图
+ 通过DFS来判断，当从一个节点dfs完的所有连通分量不等于节点总量的时候，就不是连通图。

## 代码

+ 由于使用Python可以AC，因此只放了Python的解。
+ 注意，不是每次都能AC，要多提交多试几次。

```python
num_vertice, num_edge = list(map(int, input().split()))
edges = [[] for _ in range(num_vertice + 1)]

# 道路的录入
for _ in range(num_edge):
    a, b = list(map(int, input().split()))
    edges[a].append(b)
    edges[b].append(a)

# 计算度和奇数度的节点个数
degree = [len(edges[i]) for i in range(num_vertice + 1)]
odd = len([i for i in degree if i % 2 == 1])

# dfs
count = 0
visited = [False for i in range(num_vertice + 1)]
visited[1] = True
def dfs(i):
    global count
    count += 1
    for neighbor in edges[i]:
        if not visited[neighbor]:
            visited[neighbor] = True
            dfs(neighbor)

# 输出结果
print(" ".join(list(map(str, degree[1:]))))
if odd == 0:
    dfs(1)
    if count == num_vertice:
        print("Eulerian")
    else:
        print("Non-Eulerian")
elif odd == 2:
    dfs(1)
    if count == num_vertice:
        print("Semi-Eulerian")
    else:
        print("Non-Eulerian")
else:
    print("Non-Eulerian")

```

