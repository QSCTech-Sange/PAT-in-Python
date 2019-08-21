---
title: <Python><PAT> 1018 Public Bike Management
date: 2019-08-15 17:54:32
tags: 
- PAT
- 题解
- DFS
- Dijkstra
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 高难度，大坑题。Dijkstra + dfs 或者 dfs 遍历
---

## Public Bike Management

There is a public bike service in Hangzhou City which provides great convenience to the tourists from all over the world. One may rent a bike at any station and return it to any other stations in the city.

The Public Bike Management Center (PBMC) keeps monitoring the real-time capacity of all the stations. A station is said to be in **perfect** condition if it is exactly half-full. If a station is full or empty, PBMC will collect or send bikes to adjust the condition of that station to perfect. And more, all the stations on the way will be adjusted as well.

When a problem station is reported, PBMC will always choose the shortest path to reach that station. If there are more than one shortest path, the one that requires the least number of bikes sent from PBMC will be chosen.

![example](https://images.ptausercontent.com/213)

The above figure illustrates an example. The stations are represented by vertices and the roads correspond to the edges. The number on an edge is the time taken to reach one end station from another. The number written inside a vertex $S$ is the current number of bikes stored at $S$. Given that the maximum capacity of each station is 10. To solve the problem at $S_3$, we have 2 different shortest paths:

1. PBMC -> $S_1$ -> $S_3$. In this case, 4 bikes must be sent from PBMC, because we can collect 1 bike from $S_1$ and then take 5 bikes to $S_3$, so that both stations will be in perfect conditions.
2. PBMC -> $S_2$ -> $S_3$. This path requires the same time as path 1, but only 3 bikes sent from PBMC and hence is the one that will be chosen.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 4 numbers: $C_{max} (≤100)$, always an even number, is the maximum capacity of each station; $N (≤500)$, the total number of stations; $S_p$, the index of the problem station (the stations are numbered from 1 to $N$, and PBMC is represented by the vertex 0); and $M$, the number of roads. The second line contains $N$ non-negative numbers $C_i (i=1,⋯,N)$ where each $C_i$ is the current number of bikes at $S_i$ respectively. Then $M$ lines follow, each contains 3 numbers: $S_i$, $S_j$, and $T_{ij}$ which describe the time $T_{ij}$ taken to move between stations $S_i$ and $S_j$. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print your results in one line. First output the number of bikes that PBMC must send. Then after one space, output the path in the format: 0−>$S_1$−>⋯−>$S_p$. Finally after another space, output the number of bikes that we must take back to PBMC after the condition of $S_p$ is adjusted to perfect.

Note that if such a path is not unique, output the one that requires minimum number of bikes that we must take back to PBMC. The judge's data guarantee that such a path is unique.

### Sample Input:

```in
10 3 3 5
6 7 0
0 1 1
0 2 1
0 3 3
1 3 1
2 3 1
```

### Sample Output:

```out
3 0->2->3 0
```



## 思路

此题极坑，有很多坑点：

+ 你在去问题车站的路上要把所有车站都调整到最佳状态，少了补充，多的带回
+ 不可以再回来路上调整
+ 路是双向的
+ 两个路线的比较维度其实有三个
  1. 花费时间
  2. 带过去的车数量
  3. 带回的车的数量
+ 不可以只用Dijkstra，因为这一个车站的最优点不一定是下一个车站的最优点，例如在在这个车站的有两条路线，他们里程相等，一个需要带3辆拿回3辆，一个需要带4辆拿回5辆，这时候Dijkstra会选择一个路线。如果下个车站就是终点，它缺5辆车，选第一个路线就是带5辆拿回0辆，选第二个路线就是带4辆不用拿回。这样子就是第二种路线最优了。Dijkstra 仅仅考虑目前的最优状态
+ 所以只能使用DFS。很多博主的答案是先Dijkstra算最短路线，再DFS求带走和带回的车，我认为这多此一举了，直接DFS就可以了
+ 关于自行车数量和带回带走，有一些小技巧可以简化分析。方法是这样
  + 给每个站点已有的自行车数量标记为已有数量 - 完美状态下的数量
  + 这样，是正的就需要带走，是负的就需要给他带来
  + 遍历的时候，用目前的take带走总数加上修改过的站点自行车数
    + 如果修改过的自行车数是负的，那么就需要以前的take来填补，所以加上没毛病
    + 如果修改过的自行车数是正的，那么需要加上的更多
  + 如果目前的take加上后变成了负数，说明以前的不够填补，那么需要send += take，即要多带车了，同时把take置0



## 代码

### Python3

使用Python3，最后一个点会超时，所以建议使用C++。可以使用Python3来理解算法。

#### 数据结构

+ stations[]是保存所有车站的列表，一个车站也是一个列表
  + station 的第一个参数表示从0点到这个车站的最短距离
  + station 的第二个参数表示这个站点的车子数量
  + station 的第三个参数表示这个站点的所有路线，是一个列表。
    + 一个路线是一个元祖，
      + 第一项为目的地，
      + 第二项为时间。
  + station 的第四个参数表示这个站点有没有被访问过
+ best是目前已有最佳情况，temp是目前计算中的情况
+ 对于best列表和temp列表，第一项是时间，第二项是send，第三项是take，第四项是path

#### 算法

其实就是一个深搜而已

+ 如果到了终点，比较需不需要更新目前最佳的best。
+ 如果不到终点，记录好现在的状态，遍历这个点的下一个点。
+ 这个点结束了要回溯。

```python
from copy import deepcopy

# 定义深度优先搜索的函数
def dfs(start):
    global temp, best, problem_station_index, stations
    if temp[0] > best[0]:
        return
    if start == problem_station_index and (temp[0] < best[0] or (
            temp[0] == best[0] and (temp[1] < best[1] or (temp[1] == best[1] and temp[2] < best[2])))):
        best = temp
        return

    for i in stations[start][2]:
        to, time = i[0], i[1]
        if not stations[to][3] and stations[to][0] >= stations[start][0] + time:
            # 这个点的目前最佳时间比不上新路径的时间，那么需要考虑是否要更新
            stations[to][0] = stations[start][0] + time
            stations[to][3] = True
            back = deepcopy(temp)	# 记录当前状态，用深拷贝是因为temp最后一项是列表
            temp[0] += time
            temp[2] += stations[to][1]
            if temp[2] < 0:
                temp[1] -= temp[2]
                temp[2] = 0
            temp[3].append(to)
            dfs(to)		# 进入下一个车站
            stations[to][3] = False		# 回溯
            temp = deepcopy(back)

# 元数据读入
capacity, stations_num, problem_station_index, roads_num = list(map(int, input().split()))
bike_info = input().split()
# 生成车站列表
stations = [[0, 0, [], False]] + [[9999, int(i) - capacity // 2, [], False] for i in bike_info]
# 读入路和时间
for _ in range(roads_num):
    a = input().split()
    stations[int(a[0])][2].append((int(a[1]), int(a[2])))
    stations[int(a[1])][2].append((int(a[0]), int(a[2])))

best = [9999, 9999, 9999, []]
temp = [0, 0, 0, []]
# 深度优先搜索
dfs(0)
# 输出结果
print(best[1], end=' 0')
for i in best[3]:
    print("->" + str(i), end='')
print(" " + str(best[2]))
```
### C++

算法与数据结构的内容和Python类似，不再赘述。

```c++
#include <vector>
#include <iostream>

#define INF 99999

using namespace std;

// 车站
struct station {
    int shortest_time_from_0 = INF;
    int bike = 0;
} stations[501];

// 道路
struct road {
    int to;
    int time;

    road(int to, int time) : to(to), time(time) {};
};

vector<road> roads[501];

int capacity, stations_num, problem_station_index, roads_num;

// 标记有没有访问过
bool visited[501];

// 带best是最终结果，带temp的是运算中结果
vector<int> best_path;
vector<int> temp_path;
int best_time = INF;
int best_take = INF;
int best_send = INF;
int temp_time = 0;
int temp_send = 0;
int temp_take = 0;

void dfs(int from) {
    // 一旦这条路时间大于目前最佳路时间，直接跳过
    if (temp_time > best_time)
        return;
    // 如果已经深入到了问题车站，判断要不要更新最优情况
    if (from == problem_station_index) {
        if (temp_time < best_time || (temp_time == best_time && temp_send < best_send) ||
            (temp_time == best_time && temp_send == best_send && temp_take < best_take)) {
            best_time = temp_time;
            best_take = temp_take;
            best_send = temp_send;
            best_path = temp_path;
        }
    }

    // 对from站点的每一个下一级站点进行遍历
    for (int i = 0; i < roads[from].size(); i++) {
        // 记录下一级站点的号码以及从这点到下一点的时间
        int to = roads[from][i].to;
        int time = roads[from][i].time;
        // shortest_time_from_0 这个变量利用动态规划和dijkstra算法的思想，记录从起点到这个车站的所有路径中的最小时间
        // 如果时间少了，就存在更新最优解的可能
        // 这是这道题的坑点，不能纯用dijkstra，因为send和take必须到终点才能判断
        if (!visited[to] && stations[to].shortest_time_from_0 >= stations[from].shortest_time_from_0 + time) {
            // 记录目前的记录，便于回溯
            int back_time = temp_time;
            int back_send = temp_send;
            int back_take = temp_take;
            // 更新temp_take和temp_send
            // 先全加在take上，因为如果是负数，相当于从以前多拿的来填补现在缺的，没毛病
            temp_take += stations[to].bike;
            // 如果take小于0说明前面拿的不够填了，只能多带
            if (temp_take < 0) {
                temp_send -= temp_take;
                temp_take = 0;
            }
            // 更新time,记录访问过，以及加入到路径当中
            temp_time += time;
            stations[to].shortest_time_from_0 = stations[from].shortest_time_from_0 + time;
            visited[to] = true;
            temp_path.emplace_back(to);
            // 进入深层
            dfs(to);
            // 回溯
            visited[to] = false;
            temp_time = back_time;
            temp_take = back_take;
            temp_send = back_send;
            temp_path.pop_back();
        }
    }
}


int main() {
    // 读入元数据
    cin >> capacity >> stations_num >> problem_station_index >> roads_num;
    // 读入自行车在每个车站的数量，这里将数量记为“当前数量-完美状态下的数量”
    // 为负表示，需要给它send，为正表示需要给它take
    for (int i = 1; i <= stations_num; i++) {
        cin >> stations[i].bike;
        stations[i].bike -= capacity / 2;
    }
    // 更新第一个车站的距离为0
    stations[0].shortest_time_from_0 = 0;
    // 读入道路数据，注意是无向图
    for (int i = 0; i < roads_num; i++) {
        int from, to, time;
        cin >> from >> to >> time;
        roads[from].emplace_back(to, time);
        roads[to].emplace_back(from, time);
    }
    // 开始从根部深度优先遍历
    dfs(0);
    // 输出结果
    cout << best_send << " 0";
    for (int i = 0; i < best_path.size(); i++) {
        cout << "->" << best_path[i];
    }
    cout << " " << best_take << endl;
}
```