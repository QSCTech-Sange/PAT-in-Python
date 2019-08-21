---
title: <Python><PAT> 1087 All Roads Lead to Rome
date: 2019-08-15 19:39:25
tags: 
- PAT
- 题解
- DFS
- Dijkstra
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 比较难的DFS题，需要一定的思考。
---

## All roads lead to ROME

Indeed there are many different tourist routes from our city to Rome. You are supposed to find your clients the route with the least cost while gaining the most happiness.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive integers $N (2≤N≤200)$, the number of cities, and $K$, the total number of routes between pairs of cities; followed by the name of the starting city. The next $N−1$ lines each gives the name of a city and an integer that represents the happiness one can gain from that city, except the starting city. Then $K$ lines follow, each describes a route between two cities in the format `City1 City2 Cost`. Here the name of a city is a string of 3 capital English letters, and the destination is always `ROM` which represents Rome.

### Output Specification:

For each test case, we are supposed to find the route with the least cost. If such a route is not unique, the one with the maximum happiness will be recommended. If such a route is still not unique, then we output the one with the maximum average happiness -- it is guaranteed by the judge that such a solution exists and is unique.

Hence in the first line of output, you must print 4 numbers: the number of different routes with the least cost, the cost, the happiness, and the average happiness (take the integer part only) of the recommended route. Then in the next line, you are supposed to print the route in the format `City1->City2->...->ROM`.

### Sample Input:

```in
6 7 HZH
ROM 100
PKN 40
GDN 55
PRS 95
BLN 80
ROM GDN 1
BLN ROM 1
HZH PKN 1
PRS ROM 2
BLN HZH 2
PKN GDN 1
HZH PRS 1
```

### Sample Output:

```out
3 3 195 97
HZH->PRS->ROM
```



## 思路

+ 城市可以用哈希表来表示，键就是字符串。值映射成你想要的。
+ 这道题Dijkstra算法、Bellman-Ford算法、SPFA算法、深度优先遍历都能做
+ 但是我推荐直接使用深度优先遍历，方便快捷
+ 因为有了happy的存在，所以只使用Dijkstra是不能做的。



## 代码

这道题因为使用Python能AC并且不会被卡时间，因此只留了Python代码。

#### 数据结构

+ 所有字典的键都是城市名

+ happy 字典值是幸福度

+ roads 的值是元祖列表，记录从这个城出发的所有路径
  + 元祖第一项是到达城市
  + 第二项是花费

+ best的值是一个列表，代表目前这个地点最佳状态时的
  + 最少总cost
  + 最大总happy
  + 最少cost的路径数量
  + 这条路径上的城市们
+ temp系列变量是对dfs遍历过程当中的，对于这一个城市这一条路上来说的
  + temp_cost 是到这个城的已用总cost
  + temp_happy 是到这个城的路线总happy
  + temp_path 是到这个城的路线，是一个列表

#### 算法

+ 使用深度优先遍历
+ 遍历每个城路线上的下一个城
+ 最后到了ROM城比较要不要更新best
+ 不要忘了回溯

```python
from collections import defaultdict
from copy import deepcopy
# 输入
a = input().split()
cities_num, routes_num, start_city = int(a[0]), int(a[1]), a[2]
# 数据结构定义
happy = defaultdict(int)
roads = defaultdict(list)
visited = defaultdict(int)
best = [999999, -99999, 0, []]

for _ in range(cities_num - 1):
    a = input().split()
    happy[a[0]] = int(a[1])
for _ in range(routes_num):
    a = input().split()
    roads[a[0]].append((a[1], int(a[2])))
    roads[a[1]].append((a[0], int(a[2])))


def dfs(city, temp_cost, temp_happy, temp_path):
    if city == 'ROM':
        if best[0] > temp_cost:
            best[0] = temp_cost
            best[1] = temp_happy
            best[2] = 1
            best[3] = deepcopy(temp_path)
        elif temp_cost == best[0]:
            best[2] += 1
            if best[1] < temp_happy:
                best[1] = temp_happy
                best[3] = deepcopy(temp_path)
            elif best[1] == temp_happy:
                if len(best[3]) > len(temp_path):
                    best[3] = deepcopy(temp_path)

    for to, cost in roads[city]:
        if visited[to] == 0:
            visited[to] = 1
            temp_path.append(to)
            dfs(to, cost + temp_cost, happy[to] + temp_happy, temp_path)
            visited[to] = 0	# 回溯
            temp_path.pop()

# 开始遍历
dfs(start_city, 0, 0, [])
# 输出
print(best[2], best[0], best[1], best[1] // len(best[3]))
print('->'.join([start_city] + best[3]))
```

