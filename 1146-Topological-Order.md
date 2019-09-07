---
title: <Python><PAT> 1146 Topological Order
date: 2019-09-06 19:45:36
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 这题你得懂拓扑排序，不然就凉透了。
---

# 题目

This is a problem given in the Graduate Entrance Exam in 2018: Which of the following is NOT a topological order obtained from the given directed graph? Now you are supposed to write a program to test each of the options.

![topo.jpg](https://images.ptausercontent.com/5d35ed2a-4d19-4f13-bf3f-35ed59cebf05.jpg)

### Input Specification:

Each input file contains one test case. For each case, the first line gives two positive integers N (≤ 1,000), the number of vertices in the graph, and M (≤ 10,000), the number of directed edges. Then M lines follow, each gives the start and the end vertices of an edge. The vertices are numbered from 1 to N. After the graph, there is another positive integer K (≤ 100). Then K lines of query follow, each gives a permutation of all the vertices. All the numbers in a line are separated by a space.

### Output Specification:

Print in a line all the indices of queries which correspond to "NOT a topological order". The indices start from zero. All the numbers are separated by a space, and there must no extra space at the beginning or the end of the line. It is graranteed that there is at least one answer.

### Sample Input:

```in
6 8
1 2
1 3
5 2
5 4
2 3
2 6
3 4
6 4
5
1 5 2 3 6 4
5 1 2 6 3 4
5 1 2 3 6 4
5 2 1 6 3 4
1 2 3 4 5 6
```

### Sample Output:

```out
3 4
```

# 题解

## 思路

+ 看题目你可能能理解拓扑排序是什么意思
+ 但是要做出来还是很困难的
+ 如果你学过的话，就很方便了
+ 每个拓扑排序是这样的
  + 找到一个节点，它只作为起点，没有被当作终点
  + 删掉这个节点，包括它相邻的所有路径
  + 然后再找一个只作为起点没有被当作终点的路径
  + 循环往复
+ 所以为了解决这个问题，主要有两方面
  + 如何找到只有作为起点没有把作为终点的节点？
    + 我们需要一个数组来记录“入度”，一个节点的入度即有多少个节点以这个节点为终点。
  + 如何删掉这个节点？
    + 我们对这个节点的所有邻居，使它们的入度减一即可
+ 然后就迎刃而解了。

## 数据结构

+ end 是一个数组，记录一个节点能通向的所有邻居节点
  + 下标是id
  + 值是一个列表，存放以这个节点为起点能到达的所有邻居节点
+ start 是一个数组，记录入度
  + 下标是id
  + 值是入度
+ ans 存放答案
+ temp_start 是对start 的拷贝，因为我们会不断修改temp_start

## 算法

+ 读数据
  + 起点的邻居里添加终点
  + 终点的入度加一
+ 开始遍历每个查询
  + 如果输入的节点入度不是0
    + 直接返回
  + 否则将这个节点的所有邻居节点入度减一

## 代码

+ 由于使用Python可以AC，因此只放了Python的解。

```python
num_vertice, num_edge = list(map(int, input().split()))
end = [set() for _ in range(num_vertice + 1)]
start = [0 for _ in range(num_vertice + 1)]

for _ in range(num_edge):
    a, b = list(map(int, input().split()))
    end[a].add(b)
    start[b] += 1

ans = []
for i in range(int(input())):
    temp_start = start.copy()
    for node in list(map(int,input().split())):
        if temp_start[node] != 0:
            ans.append(i)
            break
        for neighbor in end[node]:
            temp_start[neighbor] -= 1

print(" ".join(list(map(str, ans))))

```

