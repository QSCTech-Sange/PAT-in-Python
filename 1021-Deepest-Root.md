---
title: <Python><PAT> 1021 Deepest Root
date: 2019-08-22 13:06:09
tags: 
- PAT
- 题解
- 树
- DFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 充满挑战性的DFS
---

# 题目

A graph which is connected and acyclic can be considered a tree. The height of the tree depends on the selected root. Now you are supposed to find the root that results in a highest tree. Such a root is called **the deepest root**.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤104) which is the number of nodes, and hence the nodes are numbered from 1 to *N*. Then *N*−1 lines follow, each describes an edge by given the two adjacent nodes' numbers.

### Output Specification:

For each test case, print each of the deepest roots in a line. If such a root is not unique, print them in increasing order of their numbers. In case that the given graph is not a tree, print `Error: K components` where `K` is the number of connected components in the graph.

### Sample Input 1:

```in
5
1 2
1 3
1 4
2 5
```

### Sample Output 1:

```out
3
4
5
```

### Sample Input 2:

```in
5
1 3
1 4
2 5
3 4
```

### Sample Output 2:

```out
Error: 2 components
```

# 题解

## 思路

+ 首先理解题意
+ 它给你N个节点和N-1个路（数量很重要！）
+ 如果这几个节点没有环，要打印出拥有最深的叶子的那些根节点（其实这种情况下根和叶子都得算上），否则，输出连通分量个数
+ 它都提示你连通分量个数了，你再往环的方向想，就有点憨了。因为只给定N个节点和N-1条路，所有要是想没有环，那必须所有的节点都连在一起，即只有一个连通分量。要是有一条路导致了环的出现，就代表只有N-2条有效的路，那想必就是有多个连通分量了。
+ 所以转换为先求连通分量的个数，超过一的直接按格式报错。
+ 那么如何找最深的根节点呢？
+ 如果对每个节点都DFS一遍完整的图。那复杂度是巨大的。其实，只需要DFS两遍。这便是这道题的难点和核心所在。非常巧妙的一种算法。
+ 首先，对随便抓一个节点，先DFS一遍。已经出现了对于这个节点来说，最深的那几个叶子节点。
+ 然后对那几个叶子节点再抓一个DFS一遍，找到属于它的最深的叶子节点。
+ 它们的并集，就是最终解。
+ 为什么这个算法是有效的呢？
+ 想象一下，如果在连通图里，有一条最最最长的路
+ 那么这条路的两端是要被添加进答案里的
+ 如果我们随机挑选一个节点，就落在这条路上（因为是连通图）。
+ 对那个点来说，这条路上的起点或终点一定有一个是那个随机点最深的叶子。因为如果有更深的叶子的话，那么那个叶子到起点或终点的路才是最长的路。
+ 所以，对那个节点最深的叶子节点们再进行DFS，就可以得到路上的起点和终点们了。
+ 算法的核心在于减少DFS的次数，从N次减少到了2次，直接将复杂度从平方减少到了线性。

## 数据结构

+ num_nodes 是节点的个数
+ roads是一个列表
  + 下标是起始城市
  + 值是一个列表，代表下标城市能到的所有城市的集合
+ count 来记录连通分量个数
+ visited 来记录每个节点有没有被访问过
+ maxheight记录已有的最大深度
+ maxnodes记录已有的最大深度的节点
+ ans 记录答案

## 算法

+ 深度优先遍历
  + 对一个节点的高度，比较它和最大高度，然后做相应更新
  + 设这个点为访问过
  + 对这个点的所有邻居进行遍历
+ 计算连通分量
  + 对每一个没被访问过的点
    + 如果它没有被访问过，开始深度优先遍历它
    + 即把这个点能到达的所有点设为已经访问过
    + 连通分量加一
+ 计算最深的节点们
  + 先记录刚刚在计算连通分量的时候，最深的节点们，以及从中随机取一个节点。
  + 设所有节点都是没访问过
  + 对随机取的节点再进行深度优先遍历，得到一堆最深的节点们
  + 取两个最深的节点的交集
  + 记得排序！
+ 输出

## 代码

因为使用Python能AC，所以只放了AC的代码。

```python
# 数据结构初始化与信息读入
num_nodes = int(input())
roads = [[] for _ in range(num_nodes + 1)]
for _ in range(num_nodes - 1):
    info = list(map(int, input().split()))
    roads[info[0]].append(info[1])
    roads[info[1]].append(info[0])
visited = [False for i in range(num_nodes + 1)]
maxheight = 0
max_nodes = []

# 深度优先遍历
def dfs(node, height):
    global maxheight
    if height > maxheight:
        max_nodes.clear()
        max_nodes.append(node)
        maxheight = height
    elif height == maxheight:
        max_nodes.append(node)
    visited[node] = True
    for i in roads[node]:
        if not visited[i]:
            dfs(i, height + 1)
            
# 计算连通分量,第一轮DFS
count = 0
for i in range(1, num_nodes + 1):
    if not visited[i]:
        dfs(i, 1)
        count += 1

# 输出与进行第二次DFS
if count >= 2:
    print("Error: %d components" % count)
else:
    ans = set(max_nodes)
    maxheight = 0
    visited = [False for _ in range(num_nodes + 1)]
    dfs(max_nodes[0], 1)
    ans = ans | set(max_nodes)
    ans = sorted(ans)
    for i in ans:
        print(i)

```

