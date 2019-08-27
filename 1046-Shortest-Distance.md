---
title: <Python><PAT> 1046 Shortest Distance
date: 2019-08-26 20:01:27
tags: 
- PAT
- 题解
- 动态规划
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 超全超具体！七种方法的大碰撞！
---

# 题目

The task is really simple: given *N* exits on a highway which forms a simple cycle, you are supposed to tell the shortest distance between any pair of exits.

### Input Specification:

Each input file contains one test case. For each case, the first line contains an integer $N\left(\operatorname{in}\left[3,10^{5}\right]\right)$, followed by $N$ integer distances $D_{1} D_{2} \cdots D_{N}$, where $D_i$ is the distance between the *i*-th and the (*i*+1)-st exits, and $D_N$ is between the *N*-th and the 1st exits. All the numbers in a line are separated by a space. The second line gives a positive integer $M\left( \leq 10^{4}\right)$,  with *M* lines follow, each contains a pair of exit numbers, provided that the exits are numbered from 1 to *N*. It is guaranteed that the total round trip distance is no more than $10^7$.

### Output Specification:

For each test case, print your results in *M* lines, each contains the shortest distance between the corresponding given pair of exits.

### Sample Input:

```in
5 1 2 4 14 9
3
1 3
2 5
4 1
```

### Sample Output:

```out
3
10
7
```

# 题解

## 思路

+ 题目的意思是，有一些环形站点，给你这几个站点和邻居之间的距离
+ 然后给你两个站点编号，求它们的最近距离
+ 很多种方法
+ 一种是顺着思路，正着搜一遍，反着搜一遍。遇到列表尾巴了就移到0,相当于环形了。优点是直观方便。
+ 一种是，直接把原来的距离数组复制一遍，然后就搜两次，一次从原来的begin和end搜，一次搜end到distance + begin 的距离。优点是代码少。缺点是空间复杂度高且算法没什么本质上的卵用。
+ 还有一种是动态规划法。
+ 既然题目已经提示有多次查询，那么把以前的计算记录存储起来就很自然。
+ 方法是，开一个二维数组dp，对于`dp[i][j]`的含义就是，从`i`车站沿车站号增大的方向到车站`j`所需要的路程。比如`dp[2][3]`表示从车站2开到车站3的路程，而`dp[3][2]`就是从车站3开到最后一个车站绕回第0车站再开到车站2的路程。
+ 我们初始化dp数组都是0
+ 然后读入数据
  + 遍历车站号，从0到总车站
  + 这个车站到下一个车站的距离就是读入的数据。下一个车站用`(i+1)% len(stations)` 这样的取余的方式，可以规避最后加一要回到原点的问题。
+ 接下来按照距离遍历，因为自己和自己的距离是0,自己和下一个车站的距离已经给出，所以距离从2遍历到车站数量-1
  + 每次车站`i`到车站`j`的距离就相当于车站`i`到车站`j-1`加上车站`j-1`到车站`j`的距离。这是状态转移公式。因为是按照距离从小到大计算的，所以等号右边的一定是已经计算过的。同时用老方法规避回到起点的问题。写成代码就是
  + `dp[j][(j + i) % len(dis)] = dp[j][(j + i - 1) % len(dis)] + dp[(j + i - 1) % len(dis)][(j + i) % len(dis)]`其中，`i`是遍历的距离
+ 输出的时候取`dp[i][j]`与`dp[j][i]`的较小值即可。
+ 以上三种方法都没有利用到一个信息。
+ 就是整个环的长度都是固定的，假设为S，那么正着走距离是A，反着走距离一定是S-A
+ 所以算完正的就没必要再算反着的了，直接用总和减即可
+ 这一点和前面三种方法都可以组合，形成六种方法。
+ 然而我最推荐的是这第七种方法，求和列表法，也要结合上面的那一个信息。
+ 我们设一个列表，`[d0,d1,d2,d3,d4,…,]`每一个dx代表从起始站到第x号车站的距离。那么第一项就是d0 = 0,最后一项是绕成一个环以后又回到起点的距离。
+ 那么我们求x到y的距离，实际上就是求0到y的距离减去0到x的距离，与总环长度减去（0到y的距离减去0到x的距离），即`min(_sum[b] - _sum[a], _sum[-1] - _sum[b] + _sum[a])`
+ 这种方法是复杂度最低的。

## 数据结构

+ 不同方法的数据结构参考思路。

## 算法

+ 不同方法的算法参照思路和代码。

## 代码

+ 这里展示了四种方法。

### 两倍长度法

+ 会有一个点超时。

```python
distance = list(map(int, input().split()[1:]))
distance = distance + distance
i = int(input())
for _ in range(i):
    start, end = list(map(lambda x: int(x) - 1, input().split()))
    if start > end:
        start,end = end,start
    print(min(sum([distance[i] for i in range(start, end)]), sum([distance[i] for i in range(end, len(distance)//2 + start)])))

```

### 直观法

+ 会有一个点超时。

```python
distance = list(map(int, input().split()[1:]))
i = int(input())
for _ in range(i):
    start, end = list(map(lambda x: int(x) - 1, input().split()))
    i = start
    sum = 0
    while i != end:
        sum += distance[i]
        i += 1
        if i == len(distance):
            i = 0
    i = end
    sum2 = 0
    while i != start:
        sum2 += distance[i]
        i += 1
        if i == len(distance):
            i = 0
    print(min(sum, sum2))

```

### 动态规划法 

+ 会有一个点超时。

```python
dis = list(map(int, input().split()[1:]))
dp = [[0 for _ in range(len(dis))] for _ in range(len(dis))]
for i, j in enumerate(dis):
    dp[i][(i + 1) % len(dis)] = j
for i in range(2, len(dp)):
    for j in range(len(dp)):
        dp[j][(j + i) % len(dis)] = dp[j][(j + i - 1) % len(dis)] + dp[(j + i - 1) % len(dis)][(j + i) % len(dis)]
i = int(input())
for _ in range(i):
    a, b = list(map(int, input().split()))
    print(min(dp[a-1][b-1], dp[b-1][a-1]))

```

### 求和列表法 Python

+ 最终方法。但是Python还是会超时，建议使用C++。

```python
    dis = list(map(int, input().split()[1:]))
    num = len(dis)
    _sum = [sum(dis[:i]) for i in range(num + 1)]
    i = int(input())
    for _ in range(i):
        a, b = list(map(lambda x: int(x) - 1, input().split()))
        if a > b:
            a, b = b, a
        print(min(_sum[b] - _sum[a], _sum[-1] - _sum[b] + _sum[a]))

```

### 求和列表法 C++

- 稳。

```c++
#include <iostream>

using namespace std;

int main() {
    int num_exits;
    scanf("%d",&num_exits);
    int sum[num_exits + 1];
    sum[0] = 0;
    for (int i = 1; i <= num_exits; i++) {
        scanf("%d", &sum[i]);
        sum[i] += sum[i-1];
    }
    int num_que;
    scanf("%d", &num_que);
    for (int i = 0; i < num_que; i++) {
        int a, b;
        scanf("%d %d", &a, &b);
        if (a > b)
            swap(a, b);
        printf("%d\n", min(sum[b-1] - sum[a-1], sum[num_exits] - sum[b-1] + sum[a-1]));
    }
}
```

