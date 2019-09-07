---
title: <Python><PAT> 1150 Travelling Salesman Problem
date: 2019-09-07 10:48:44
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 给你一个崭新的路径的概念
---

# 题目

The "travelling salesman problem" asks the following question: "Given a list of cities and the distances between each pair of cities, what is the shortest possible route that visits each city and returns to the origin city?" It is an NP-hard problem in combinatorial optimization, important in operations research and theoretical computer science. (Quoted from "https://en.wikipedia.org/wiki/Travelling_salesman_problem".)

In this problem, you are supposed to find, from a given list of cycles, the one that is the closest to the solution of a travelling salesman problem.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive integers *N* (2<*N*≤200), the number of cities, and *M*, the number of edges in an undirected graph. Then *M* lines follow, each describes an edge in the format `City1 City2 Dist`, where the cities are numbered from 1 to *N* and the distance `Dist` is positive and is no more than 100. The next line gives a positive integer *K* which is the number of paths, followed by *K* lines of paths, each in the format:
$$
n C_1 C_2 ... C_n
$$
where *n* is the number of cities in the list, and $C_i$'s are the cities on a path.

### Output Specification:

For each path, print in a line `Path X: TotalDist (Description)` where `X` is the index (starting from 1) of that path, `TotalDist` its total distance (if this distance does not exist, output `NA` instead), and `Description` is one of the following:

- `TS simple cycle` if it is a simple cycle that visits every city;
- `TS cycle` if it is a cycle that visits every city, but not a simple cycle;
- `Not a TS cycle` if it is NOT a cycle that visits every city.

Finally print in a line `Shortest Dist(X) = TotalDist` where `X` is the index of the cycle that is the closest to the solution of a travelling salesman problem, and `TotalDist` is its total distance. It is guaranteed that such a solution is unique.

### Sample Input:

```in
6 10
6 2 1
3 4 1
1 5 1
2 5 1
3 1 8
4 1 6
1 6 1
6 3 1
1 2 1
4 5 1
7
7 5 1 4 3 6 2 5
7 6 1 3 4 5 2 6
6 5 1 4 3 6 2
9 6 2 1 6 3 4 5 2 6
4 1 2 5 1
7 6 1 2 5 4 3 1
7 6 3 2 5 4 1 6
```

### Sample Output:

```out
Path 1: 11 (TS simple cycle)
Path 2: 13 (TS simple cycle)
Path 3: 10 (Not a TS cycle)
Path 4: 8 (TS cycle)
Path 5: 3 (Not a TS cycle)
Path 6: 13 (Not a TS cycle)
Path 7: NA (Not a TS cycle)
Shortest Dist(4) = 8
```

# 题解

## 思路

+ 给你一张图和一个路径
+ 然你判断路径是TS simple还是TS cycle 还是Not a cycle
+ 前两者还要记录最小值
+ 难点主要是这三者的判断依据
  + TS simple
    + 遍历每座城市，不包含重复的（首尾不算）
  + TS cycle 
    + 遍历每座城市，可以包含重复的
  + Not a cycle
    + 相邻两个城市之间没有路，或者没有访问完所有的节点
+ 我们可以使用集合来辅助判断
  + 如果路径的集合的长度，小于所有节点的数量
    + 说明没有访问过所有节点，是Not a cycle
  + 否则，访问过了所有的节点
    + 如果路径的长度 > 节点的数量 + 1
      + 说明访问过了重复的城市，是TS cycle
    + 否则，正好是每个节点走了一遍
      + 是TS simple
+ 同时，如果路径相邻两座城之间没有路或首尾不一致，也是Not a cycle

## 数据结构

+ roads 记录道路，用邻届矩阵的形式记录。比如`roads[2][5]=2` 代表从2节点到5的节点的距离为2。如果距离是-1，说明这个路不存在。
+ min_dist 是最短的距离
  min_index  是最短的距离的查询下标
+ index 是查询下标
+ dist 是这次查询的距离

## 算法

+ 记录所有的道路
+ 初始化最小距离为999999
+ 对每一个查询，进入judge()函数
  + 初始化dist 为0
  + 遍历路径上的每条路
    + 并把距离加到dist里
    + 如果遇到了-1,即无路可走，直接打印不是TS并返回
  + 如果都走完了，那么先查看它是不是走完了每个节点以及是否首尾是同一个节点
    + 不是的话
      + 就返回不是TS
    + 是的话
      + 根据路径的长度判断是不是simple的
      + 然后判断是不是最小距离
      + 输出

## 代码

+ 由于使用Python可以AC，因此只放了Python的题解。

```python
num_vertice, num_roads = list(map(int, input().split()))
roads = [[-1 for _ in range(num_vertice+1)] for _ in range(num_vertice+1)]
for _ in range(num_roads):
    a, b, dis = list(map(int, input().split()))
    roads[a][b] = dis
    roads[b][a] = dis


def judge(path, index):
    global min_dist,min_index
    dist = 0
    for i in range(1,len(path)):
        if roads[path[i - 1]][path[i]] == -1:
            print("Path %d: NA (Not a TS cycle)" % index)
            return 0
        dist += roads[path[i-1]][path[i]]
    if len(set(path)) != num_vertice or path[0] != path[-1]:
        cycle = "(Not a TS cycle)"
    else:
        if len(path) == num_vertice + 1:
            cycle = "(TS simple cycle)"
        else:
            cycle = "(TS cycle)"
        if dist < min_dist:
            min_dist = dist
            min_index = index
    print("Path %d: %d" % (index, dist), cycle)


min_dist = 99999999
min_index = 0
for i in range(int(input())):
    path = list(map(int, input().split()[1:]))
    judge(path, i + 1)

print("Shortest Dist(%d) = %d" % (min_index, min_dist))

```

