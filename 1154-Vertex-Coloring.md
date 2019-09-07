---
title: <Python><PAT> 1154 Vertex Coloring
date: 2019-09-07 16:47:03
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 图与颜色
---

# 题目

A **proper vertex coloring** is a labeling of the graph's vertices with colors such that no two vertices sharing the same edge have the same color. A coloring using at most *k* colors is called a (proper) **k-coloring**.

Now you are supposed to tell if a given coloring is a proper *k*-coloring.

### Input Specification:

Each input file contains one test case. For each case, the first line gives two positive integers *N* and *M* (both no more than 104), being the total numbers of vertices and edges, respectively. Then *M* lines follow, each describes an edge by giving the indices (from 0 to *N*−1) of the two ends of the edge.

After the graph, a positive integer *K* (≤ 100) is given, which is the number of colorings you are supposed to check. Then *K* lines follow, each contains *N* colors which are represented by non-negative integers in the range of **int**. The *i*-th color is the color of the *i*-th vertex.

### Output Specification:

For each coloring, print in a line `k-coloring` if it is a proper `k`-coloring for some positive `k`, or `No` if not.

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
4
0 1 0 1 4 1 0 1 3 0
0 1 0 1 4 1 0 1 0 0
8 1 0 1 4 1 0 5 3 0
1 2 3 4 5 6 7 8 8 9
```

### Sample Output:

```out
4-coloring
No
6-coloring
No
```

# 题解

## 思路

+ 挺水的一题
+ 给你一张图
+ 给你每个点的颜色
+ 让你判断是不是每个路径的两个端点颜色不同，并输出总共用了几种颜色。
+ 直接把路径存储起来，然后用集合判定颜色数量即可。

## 数据结构

+ roads 存放所有的路，是一个列表
  + 一条路由一个二元素列表组成，即两个端点
+ color 是每个节点的颜色

## 算法

+ 读取道路信息
+ 读取颜色信息
+ 遍历每条路
+ 如果有一条路两个端点颜色相同，输出No，break
+ 否则，输出有几种颜色。取集合取长度即可。

## 代码

+ 由于使用Python可以AC，因此只放了Python的题解

```python
num_vertice, num_edges = list(map(int, input().split()))
roads = [None for _ in range(num_edges)]
for i in range(num_edges):
    a, b = list(map(int, input().split()))
    roads[i] = [a, b]
for _ in range(int(input())):
    color = list(map(int, input().split()))
    for road in roads:
        if color[road[0]] == color[road[1]]:
            print("No")
            break
    else:
        print("%d-coloring" % len(set(color)))

```

