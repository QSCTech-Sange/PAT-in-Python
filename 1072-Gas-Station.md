---
title: <Python><PAT> 1072 Gas Station
date: 2019-09-01 15:26:07
tags: 
- PAT
- 题解
- Dijkstra
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 单源最短路径的计算
---

# 题目

A gas station has to be built at such a location that the minimum distance between the station and any of the residential housing is as far away as possible. However it must guarantee that all the houses are in its service range.

Now given the map of the city and several candidate locations for the gas station, you are supposed to give the best recommendation. If there are more than one solution, output the one with the smallest average distance to all the houses. If such a solution is still not unique, output the one with the smallest index number.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 4 positive integers: $N(\leq 10 ^ 3)$, the total number of houses; $M(\leq 10)$, the total number of the candidate locations for the gas stations; $K(\leq 10 ^ 4)$, the number of roads connecting the houses and the gas stations; and $D_s$, the maximum service range of the gas station. It is hence assumed that all the houses are numbered from 1 to *N*, and all the candidate locations are numbered from `G`1 to `G`*M*.

Then *K* lines follow, each describes a road in the format

```
P1 P2 Dist
```

where `P1` and `P2` are the two ends of a road which can be either house numbers or gas station numbers, and `Dist` is the integer length of the road.

### Output Specification:

For each test case, print in the first line the index number of the best location. In the next line, print the minimum and the average distances between the solution and all the houses. The numbers in a line must be separated by a space and be accurate up to 1 decimal place. If the solution does not exist, simply output `No Solution`.

### Sample Input 1:

```in
4 3 11 5
1 2 2
1 4 2
1 G1 4
1 G2 3
2 3 2
2 G2 1
3 4 2
3 G3 2
4 G1 3
G2 G1 1
G3 G2 2
```

### Sample Output 1:

```out
G1
2.0 3.3
```

### Sample Input 2:

```in
2 1 2 10
1 G1 9
2 G1 20
```

### Sample Output 2:

```out
No Solution
```

# 题解

## 思路

+ 本体上，还是一副有权无向图
+ 让你求一些节点到另外一些节点的最短距离
+ 也就是使用Dijkstra，做一些简单的变换
+ 首先是节点的表示
+ 我们把前面的房子按照0到N-1的标记，后面的N到M-1的位置留给加油站。例如，0代表房子id的1,而N代表加油站1，N+1代表加油站2,以此类推
+ 还有就是更新节点的方式有些独特
+ 要选取的节点是，到达每一个住户的距离中的最短距离，要使得它尽可能大，同时也要保证每个住户都在服务范围当中。
+ 掌握了这两点，同时使用Dijkstra算法，边可迎刃而解。

## 数据结构

+  因为采取了思路中所述的节点表示法，所以下文的车站，加油站，服务站，服务楼有时候可以混用。
+ minimal_distance, average_distance, best_gas 这三者分别是目前最优解的
  + 到最近一个居民楼的距离
  + 到所有居民楼的距离
  + 最合适的加油站站点号
+ num_hou, num_gas, num_roa 是
  + 居民楼数量
  + 加油站数量
  + 道路数量
+ serve_range 是服务范围
+ roads 记载所有的道路，是一个二维数组。
  + `roads[3][5] = 7` 表示从三号位置到五号位置的距离为7
+ 在一次dijkstra中
  + visited 记录车站是否被访问过
  + dis 是一个列表，记录这个加油站到所有地方的距离
  + temp_minimal, temp_average 记录目前计算中的这个车站的临时
    - 到最近一个居民楼的距离
    - 到所有居民楼的距离
  + closest_station,closest_distance 是目前寻找到的没有被访问过的离加油站最近的位置和距离

## 算法

+ 首先读入数据
  + 当我们读到车站为G开头的字符串的时候，要将下标改成其后面的数字 + 居民楼的数量 -1。
  + 否则，下标为读到的数字 - 1
+ 遍历每一个车站，对其进行Dijkstra，这个算法的核心就是先设立起点到每个点的距离都是无限大，然后每次选择所有距离当中最小的那个距离，并更新它的邻居。
  + 设dis为到自己的距离为0,到其他车站的距离都为无限大
  + 设visit为，自己为True，其他都是False，没访问过
  + 遍历（加油站+居民楼总数）的次数
    + 每次都找到一个没有被访问过且离加油站最近的站点（一开始即加油站本身）
    + 如果找不到了就直接break
    + 找到了以后，将它的visit设为True
    + 对这个站点的所有邻居，更新其dis为原来的值和新值中的最小值。新值就是原来到这个车站的距离加上这个车站到这个邻居的距离。
  + 上轮全部完成后，dis就记录了加油站到每个站点的最短距离。
  + 遍历所有房子，记录到所有房子的距离中的最小距离，以及平均距离
  + 判断是否要更新
+ 输出即可

## 代码

+ 使用Python会有一个点超时，因此也放了C++的解。

### Python

```python
def dijkstra(gas):
    # Dijkstra 初始化
    global minimal_distance, average_distance, best_gas
    temp_minimal, temp_average = 99999, 0
    visited = [False for _ in range(num_hou + num_gas)]
    distance = [99999 for _ in range(num_hou + num_gas)]
    distance[gas] = 0
    # 更新所有站点的dis
    for j in range(num_hou + num_gas):
        # 找到离加油站最近且未被访问过的车站
        closest_station = min([i for i in range(num_hou + num_gas) if not visited[i]], key=lambda x: distance[x])
        closest_distance = distance[closest_station]
        if closest_distance == 99999:
            break
        visited[closest_station] = True
        # 更新这个车站所有邻居的dis
        for k in range(num_gas + num_hou):
            distance[k] = min(distance[k], distance[closest_station] + roads[closest_station][k])
    # 计算temp_minimal和temp_average
    for house in range(num_hou):
        if distance[house] > serve_range:
            return
        if distance[house] < temp_minimal:
            temp_minimal = distance[house]
        temp_average += distance[house]
    temp_average = temp_average / num_hou
    # 比较是否需要更新最优解
    if temp_minimal > minimal_distance or (temp_minimal == minimal_distance and temp_average < average_distance):
        best_gas = gas
        minimal_distance = temp_minimal
        average_distance = temp_average

# 数据读入
num_hou, num_gas, num_roa, serve_range = list(map(int, input().split()))
roads = [[99999 for _ in range(num_hou + num_gas)] for _ in range(num_hou + num_gas)]
for _ in range(num_roa):
    start, end, dist = input().split()
    start = int(start[1:]) + num_hou - 1 if start[0] == "G" else int(start) - 1
    end = int(end[1:]) + num_hou - 1 if end[0] == "G" else int(end) - 1
    roads[start][end] = int(dist)
    roads[end][start] = int(dist)
# 遍历加油站，进行Dijkstra 
minimal_distance, average_distance = 0, 0
best_gas = None
for gas in range(num_hou, num_hou + num_gas):
    dijkstra(gas)
# 输出
if best_gas:
    print("G" + str(best_gas - num_hou + 1))
    print("%.1f %.1f" % (minimal_distance, average_distance))
else:
    print("No Solution")

```

### C++

```c++
#include <iostream>
#include <string>

using namespace std;

double minimal_distance, average_distance;
int roads[1010][1010],dis[1010];
bool visited[1010];
int best_gas,num_gas, num_hou, serve_range;

int dijkstra(int gas) {
    double temp_minimal, temp_average;
    temp_minimal = 999999;
    temp_average = 0;
    for (int i = 0; i < num_gas + num_hou; i++)
        visited[i] = false;
    for (int i = 0; i < num_gas + num_hou; i++)
        dis[i] = 999999;
    dis[gas] = 0;
    for (int j = 0; j < num_gas + num_hou; j++) {
        int closest_station = -1;
        int closest_distance = 999999;
        for (int i = 0; i < num_gas + num_hou; i++) {
            if (!visited[i] && closest_distance > dis[i]) {
                closest_station = i;
                closest_distance = dis[i];
            }
        }
        if (closest_station == -1)
            break;
        visited[closest_station] = true;
        for (int k = 0; k < num_gas + num_hou; k++)
            dis[k] = min(dis[k], dis[closest_station] + roads[closest_station][k]);
    }
    for (int house = 0; house < num_hou; house++) {
        if (dis[house] > serve_range)
            return 0;
        if (dis[house] < temp_minimal)
            temp_minimal = dis[house];
        temp_average += dis[house];
    }
    temp_average = temp_average / num_hou;
    if (temp_minimal > minimal_distance || (temp_minimal == minimal_distance && temp_average < average_distance)) {
        best_gas = gas;
        minimal_distance = temp_minimal;
        average_distance = temp_average;
    }
}


int main() {
    int num_roa;
    scanf("%d%d%d%d", &num_hou, &num_gas, &num_roa, &serve_range);
    for (int i = 0; i < num_gas + num_hou; i++) {
        for (int j = 0; j < num_gas + num_hou; j++)
            roads[i][j] = 999999;
    }
    for (int i = 0; i < num_roa; i++) {
        string start, end;
        int dist;
        cin >> start >> end >> dist;
        int int_start = start[0] == 'G' ? num_hou + stoi(start.substr(1)) - 1 : stoi(start)-1;
        int int_end = end[0] == 'G' ? num_hou + stoi(end.substr(1)) - 1 : stoi(end)-1;
        roads[int_start][int_end] = dist;
        roads[int_end][int_start] = dist;
    }
    for (int gas = num_hou; gas < num_hou + num_gas; gas++)
        dijkstra(gas);
    if (best_gas != 0) {
        printf("G%d\n%.1f %.1f\n", best_gas - num_hou + 1,minimal_distance, average_distance);
    } else
        printf("No Solution\n");
}
```

