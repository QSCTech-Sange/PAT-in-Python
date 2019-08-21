---
title: <Python><PAT> 1003 Emergency
date: 2019-08-19 13:11:29
tags: 
- PAT
- 题解
- DFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 比较简单的深度优先遍历
---

# 题目

As an emergency rescue team leader of a city, you are given a special map of your country. The map shows several scattered cities connected by some roads. Amount of rescue teams in each city and the length of each road between any pair of cities are marked on the map. When there is an emergency call to you from some other city, your job is to lead your men to the place as quickly as possible, and at the mean time, call up as many hands on the way as possible.

### Input Specification:

Each input file contains one test case. For each test case, the first line contains 4 positive integers: *N* (≤500) - the number of cities (and the cities are numbered from 0 to *N*−1), *M* - the number of roads, *C*1 and *C*2 - the cities that you are currently in and that you must save, respectively. The next line contains *N* integers, where the *i*-th integer is the number of rescue teams in the *i*-th city. Then *M* lines follow, each describes a road with three integers *c*1, *c*2 and *L*, which are the pair of cities connected by a road and the length of that road, respectively. It is guaranteed that there exists at least one path from *C*1 to *C*2.

### Output Specification:

For each test case, print in one line two numbers: the number of different shortest paths between *C*1 and *C*2, and the maximum amount of rescue teams you can possibly gather. All the numbers in a line must be separated by exactly one space, and there is no extra space allowed at the end of a line.

### Sample Input:

```in
5 6 0 2
1 2 1 5 3
0 1 1
0 2 2
0 3 1
1 2 1
2 4 1
3 4 1
```

### Sample Output:

```out
2 4
```

# 题解

## 思路

+ 看题目容易得出这是一个比较基础的深度优先遍历
+ 然后就没什么思路可写了，挺常规的。

## 代码

因为使用Python3能AC，所以只写了Python3版本的。

### 数据结构

+ rescue 是一个数组
  + 下标是城市号
  + 值是这个城市的救援队数量
+ roads 是一个列表数组，包含了所有的道路
  + 下标是城市号
  + 值是一个道路列表，代表从这个城市出发的所有道路
    + 一个道路是一个元祖，包含
      + 通往城市
      + 路程长度
+ min_roads,max_rescue,min_distance 代表目前已有最优解的最少里程的道路数量，最大救援队，和最短里程
+ temp_distance, temp_rescue 代表计算过程中路程和救援队数量
+ visited 是一个集合，表示已经走过的路，避免走回头路

### 算法

+ 读取数据并添加到我们的数据结构里
+ 从起点开始深度优先遍历
+ 当遍历到终点时，判断距离是最不是最近的，以及更新相应的数据
+ 不是终点的情况下，继续深度优先遍历它的所有邻居。
+ 记得回溯！

### Python3

```python
# 信息读入
num_city, num_roads, cur_city, save_city = list(map(int, input().split()))
rescue = list(map(int, input().split()))
roads = [[] for _ in range(num_city)]
for _ in range(num_roads):
    a = list(map(int, input().split()))
    roads[a[0]].append((a[1], a[2]))
    roads[a[1]].append((a[0], a[2]))

# 定义几个变量
min_roads, max_rescue, min_distance = 0, 0, 99999
temp_distance, temp_rescue = 0, rescue[cur_city]
visited = {cur_city}

# 深度优先遍历的函数定义
def dfs(city):
    global save_city, temp_distance, min_distance, temp_distance, temp_rescue, min_roads, max_rescue, visited
    if city == save_city:# 判断是不是终点站并按需更新
        if temp_distance < min_distance:
            min_distance = temp_distance
            min_roads = 1
            max_rescue = temp_rescue
        elif temp_distance == min_distance:
            min_roads += 1
            if temp_rescue > max_rescue:
                max_rescue = temp_rescue
        return

    for next_city, distance in roads[city]:# 对这个城市的每一个邻居都走一遍dfs
        if next_city not in visited:
            visited.add(next_city)
            temp_distance += distance
            temp_rescue += rescue[next_city]
            dfs(next_city)
            temp_distance -= distance	# 这剩下三步都是用来回溯的
            temp_rescue -= rescue[next_city]
            visited.remove(next_city)

#开始dfs
dfs(cur_city)
# 打印结果
print(min_roads, max_rescue)
```

