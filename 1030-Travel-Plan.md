---
title: <Python><PAT> 1030 Travel Plan
date: 2019-08-24 10:10:39
tags: 
- PAT
- 题解
- DFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 深度优先遍历完事儿，虽然是30分的题但是真的不难。
---

# 题目

A traveler's map gives the distances between cities along the highways, together with the cost of each highway. Now you are supposed to write a program to help a traveler to decide the shortest path between his/her starting city and the destination. If such a shortest path is not unique, you are supposed to output the one with the minimum cost, which is guaranteed to be unique.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 4 positive integers *N*, *M*, *S*, and *D*, where *N* (≤500) is the number of cities (and hence the cities are numbered from 0 to *N*−1); *M* is the number of highways; *S* and *D* are the starting and the destination cities, respectively. Then *M* lines follow, each provides the information of a highway, in the format:

```
City1 City2 Distance Cost
```

where the numbers are all integers no more than 500, and are separated by a space.

### Output Specification:

For each test case, print in one line the cities along the shortest path from the starting point to the destination, followed by the total distance and the total cost of the path. The numbers must be separated by a space and there must be no extra space at the end of output.

### Sample Input:

```in
4 5 0 3
0 1 1 20
1 3 2 30
0 3 4 10
0 2 2 20
2 3 1 20
```

### Sample Output:

```out
0 2 3 3 40
```

# 题解

## 思路

+ 给定有权无向图，求一个起点到终点的最佳线路。
+ 权是(路径，消费)，有优先级
+ 可以采用Dijkstra，也可以用DFS，我选择了后者，因为方便且能AC

## 数据结构

+ roads 是一个列表，记录了所有的道路
  + 下标是城市ID
  + 值是一个列表，代表这座城的所有道路
    + 一条道路也是一个列表，包含
    + 下一个城市
    + 距离
    + 花费
+ visited 记录每个城有没有被访问过
+ route,temp_cost,temp_distance 记录目前计算中的路径，花费和距离
+ best_route,min_cost,min_distance 记录最佳状态的路径，最小花费和最小距离

## 算法

+ 就DFS呗
+ 先对起点深度优先遍历
+ 对这个城市的所有道路上的未被访问过城市，更新temp_cost,temp_distance,route，继续深度优先遍历。
+ 注意要回溯
+ 当遍历到终点时，比较是否需要更新min_cost,min_distance和min_route
+ 我这里使用了剪枝，但实际上不剪也不会超时。

## 代码

因为使用Python能AC，因此只放了Python的代码。

```python
# 数据结构初始化
num_cities, num_roads, start, destination = list(map(int, input().split()))
roads = [[] for _ in range(num_cities)]
for _ in range(num_roads):
    city_1, city_2, distance, cost = list(map(int, input().split()))
    roads[city_1].append([city_2, distance, cost])
    roads[city_2].append([city_1, distance, cost])
visited = [False for _ in range(num_cities)]
min_cost, min_distance = 999999, 999999
temp_cost, temp_distance = 0, 0
route, best_route = [start], [start]


# 深度优先遍历
def dfs(city):
    global min_cost, min_distance, temp_distance, temp_cost, destination, best_route, route
    if temp_distance > min_distance:
        return
    if city == destination:
        if temp_distance < min_distance:
            min_distance = temp_distance
            min_cost = temp_cost
            best_route = route.copy()
        elif temp_distance == min_distance and min_cost > temp_cost:
            min_cost = temp_cost
            best_route = route.copy()
        return
    for next, dis, cost in roads[city]:
        if not visited[next]:
            temp_cost += cost
            temp_distance += dis
            visited[next] = True
            route.append(next)
            dfs(next)  # DFS下一座城
            temp_cost -= cost  # 剩下几步就是回溯
            temp_distance -= dis
            visited[next] = False
            route.pop()


# DFS与给出答案
dfs(start)
print(" ".join(list(map(str, best_route + [min_distance, min_cost]))))

```

