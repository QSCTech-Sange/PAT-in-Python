---
title: <Python><PAT> 1013 Battle Over Cities
date: 2019-08-20 17:29:59
tags: 
- PAT
- 题解
- DFS
- BFS
- 并查集
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 算法难度不大，但是思路有一点难想。
---

# 题目

It is vitally important to have all the cities connected by highways in a war. If a city is occupied by the enemy, all the highways from/toward that city are closed. We must know immediately if we need to repair any other highways to keep the rest of the cities connected. Given the map of cities which have all the remaining highways marked, you are supposed to tell the number of highways need to be repaired, quickly.

For example, if we have 3 cities and 2 highways connecting $city1-city2$ and $city-city3$. Then if $city1$ is occupied by the enemy, we must have 1 highway repaired, that is the highway $city2-city3$.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 3 numbers *N* (<1000), *M* and *K*, which are the total number of cities, the number of remaining highways, and the number of cities to be checked, respectively. Then *M* lines follow, each describes a highway by 2 integers, which are the numbers of the cities the highway connects. The cities are numbered from 1 to *N*. Finally there is a line containing *K* numbers, which represent the cities we concern.

### Output Specification:

For each of the *K* cities, output in a line the number of highways need to be repaired if that city is lost.

### Sample Input:

```in
3 2 3
1 2
1 3
1 2 3
```

### Sample Output:

```out
1
0
0
```

# 题解

## 思路

+ 题目是给定一个连通图，问你如果缺少了一个城市，剩下的要补几条道路才能连通
+ 如果从道路的方向想，就会感觉很困难，无从下手
+ 但是可以换个角度想
+ 例如，有一块大陆，如果中间有一片陆地变成了水，剩下的就变成了几个岛屿
+ 如果让你求岛屿数量，那么可以使用BFS,DFS和并查集
+ 和这题有什么关系呢？
+ 这样想，如果国家变成了n个城邦，那么这n个不连通的城邦至少需要多少条道路才能联通？
+ 答案是n-1，你把它们一个一个连起来排成一列就行了，这样所有的都能相通
+ 所以题目可以转化为求去除了这个城市以后，剩下的连通的城邦数量
+ 同样可以使用BFS，DFS 与 并查集

## 数据结构

+ roads 是一个列表（数组）
  + 下标是城市id
  + 值是列表(vector)，代表这个城市联通的所有城市

## 算法

+ 面向考试的话，不建议使用并查集，代码量略大，太耗时间了。
+ BFS就是和队列配合一起进行广度优先遍历。一次性从一个节点出发，找到它的所有连通量。也就是从一个城扩散到整个城邦。
+ DFS就是和栈配合一起进行广度优先遍历。一次性从一个节点出发，找到它的所有连通量。也就是从一个城扩散到整个城邦。也可以使用递归。
+ 并查集是一个数组，下标代表城市，值代表父亲城市。没有父亲城市的一般用-1表示。
+ 有时间我会把具体内容更新到算法小品专栏，便于更深层次的学习。

## 代码

使用Python的代码最后一个点会超时，因此建议使用C++来做题。这里放上了四份代码，用不同的做法，便于融会贯通。

### Python3 BFS

```python
N, M, K = list(map(int, input().split()))
roads = [[] for i in range(N)]
for _ in range(M):
    a = input().split()
    roads[int(a[0]) - 1].append(int(a[1]) - 1)
    roads[int(a[1]) - 1].append(int(a[0]) - 1)

check = list(map(int, input().split()))
for i in check:
    count = 0
    visited = [False for _ in range(N)]
    visited[i - 1] = True
    for start, _ in enumerate(roads):
        if not visited[start]:
            count += 1
            que = [start]
            while que:
                city = que.pop()
                for destination in roads[city]:
                    if not visited[destination]:
                        visited[destination] = True
                        que.insert(0, destination)
    print(count - 1)

```

### Python3 使用递归的DFS

```python
N, M, K = list(map(int, input().split()))
roads = [[] for i in range(N)]
for _ in range(M):
    a = input().split()
    roads[int(a[0]) - 1].append(int(a[1]) - 1)
    roads[int(a[1]) - 1].append(int(a[0]) - 1)


def dfs(city):
    visited[city] = True
    for destination in roads[city]:
        if not visited[destination]:
            dfs(destination)


check = list(map(int, input().split()))
for i in check:
    count = 0
    visited = [False for _ in range(N)]
    visited[i - 1] = True
    for start, _ in enumerate(roads):
        if not visited[start]:
            count += 1
            dfs(start)
    print(count - 1)

```

### Python3 不用递归的DFS

```python
N, M, K = list(map(int, input().split()))
roads = [[] for i in range(N)]
for _ in range(M):
    a = input().split()
    roads[int(a[0]) - 1].append(int(a[1]) - 1)
    roads[int(a[1]) - 1].append(int(a[0]) - 1)

check = list(map(int, input().split()))
for i in check:
    count = 0
    visited = [False for _ in range(N)]
    visited[i - 1] = True
    for start, _ in enumerate(roads):
        if not visited[start]:
            count += 1
            stk = [start]
            while stk:
                city = stk.pop()
                for destination in roads[city]:
                    if not visited[destination]:
                        visited[destination] = True
                        stk.append(destination)
    print(count - 1)

```

### C++ 不用递归的DFS

```c++
#include <iostream>
#include <vector>
#include <stack>

using namespace std;

int main() {
    int N, M, K;
    cin >> N >> M >> K;
    vector<int> roads[N + 1];
    for (int i = 0; i < M; i++) {
        int a, b;
        cin >> a >> b;
        roads[a].push_back(b);
        roads[b].push_back(a);
    }
    for (int i = 0; i < K; i++) {
        int destroyed;
        cin >> destroyed;
        int count = 0;
        bool visited[N + 1];
        for (int j = 1; j < N + 1; j++)
            visited[j] = false;
        visited[destroyed] = true;
        for (int j = 1; j < N + 1; j++) {
            if (!visited[j])
            {
                count += 1;
                stack<int> stk;
                stk.push(j);
                visited[j] = true;
                while (!stk.empty()) {
                    int city = stk.top();
                    stk.pop();
                    for (auto k:roads[city]) {
                        if (!visited[k]) {
                            visited[k] = true;
                            stk.push(k);
                        }
                    }
                }
            }
        }
        cout << count - 1 << endl;
    }
}
```

