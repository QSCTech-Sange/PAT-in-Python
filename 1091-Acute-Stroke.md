---
title: <Python><PAT> 1091 Acute Stroke
date: 2019-08-16 09:33:04
tags: 
- PAT
- 题解
- DFS
- BFS
- 并查集
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 有点抽象的BFS遍历，难度中等。
---

## **1091** Acute Stroke

One important factor to identify acute stroke (急性脑卒中) is the volume of the stroke core. Given the results of image analysis in which the core regions are identified in each MRI slice, your job is to calculate the volume of the stroke core.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 4 positive integers: $M$, $N$, $L$ and $T$, where $M$ and $N$ are the sizes of each slice (i.e. pixels of a slice are in an $M \times N$ matrix, and the maximum resolution is 1286 by 128); $L (≤60)$ is the number of slices of a brain; and $T$ is the integer threshold (i.e. if the volume of a connected core is less than $T$, then that core must not be counted).

Then $L$ slices are given. Each slice is represented by an $M\times N$ matrix of 0's and 1's, where 1 represents a pixel of stroke, and 0 means normal. Since the thickness of a slice is a constant, we only have to count the number of 1's to obtain the volume. However, there might be several separated core regions in a brain, and only those with their volumes no less than $T$ are counted. Two pixels are **connected** and hence belong to the same region if they share a common side, as shown by Figure 1 where all the 6 red pixels are connected to the blue one.

![Figure 1](https://images.ptausercontent.com/f85c00cc-62ce-41ff-8dd0-d1c288d87409.jpg)



### Output Specification:

For each case, output in a line the total volume of the stroke core.

### Sample Input:

```in
3 4 5 2
1 1 1 1
1 1 1 1
1 1 1 1
0 0 1 1
0 0 1 1
0 0 1 1
1 0 1 1
0 1 0 0
0 0 0 0
1 0 1 1
0 0 0 0
0 0 0 0
0 0 0 1
0 0 0 1
1 0 0 0
```

### Sample Output:

```out
26
```

## 思路

题目看起来很复杂，其实可以简化一下。

+ 想象在一个二维地图中
+ 0表示水，1表示陆地
+ 你要找到这个地图中的陆地数量

提到这个问题，一般的做法是使用`DFS`或`BFS`或`并查集`，可以参照在LeetCode上[岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)这道题的题解。我建议使用`DFS`或者`BFS`来做，`并查集`代码量会比较大。

这道题复杂在

+ 陆地数量要大于一个阀值
+ 而且图像是三维的

第一个好办，第二个的话，就是要对于邻居节点作修改。二维图对于一个节点来说，会有上下所有四个邻居，而三维图呢，是六个邻居。即，除了上下左右，还有头上和脚下。要注意这三维的顺序。那么接下来就可以写代码了。

## 代码

### Python3

#### 数据结构

+ `three_d_image` 是一个三维数组，记录一个坐标的结果是1还是0
+ `visited` 是一个三维数组，记录这个坐标有没有被访问过

#### 算法

采用 `DFS` 广度优先遍历，会有两个点超时，这道题建议使用`C++`来做。

使用`BFS`也一样，无非是把栈换成队列，也是后两个点超时。

```Python
# 读入数据与数据结构定义
M, N, L, T = list(map(int, input().split()))
three_d_image = [[None for _ in range(M)] for _ in range(L)]
visited = [[[False for _ in range(N)] for _ in range(M)] for _ in range(L)]
ans = 0
for i in range(L):
    for j in range(M):
        condition = list(map(int, input().split()))
        three_d_image[i][j] = condition
# 开始 BFS
for i in range(L):
    for j in range(M):
        for k in range(N):
            # 出现了一个新块
            if three_d_image[i][j][k] == 1 and not visited[i][j][k]:
                count = 0
                stk = []
                stk.append([i, j, k])
                visited[i][j][k] = True
                while stk:
                    old = stk.pop()
                    count += 1
                    # 使用zip来遍历六个方向上的点
                    for increment in zip([1, 0, 0, -1, 0, 0], [0, 1, 0, 0, -1, 0], [0, 0, 1, 0, 0, -1]):
                        new = [x + y for x, y in zip(increment, old)]
                        # 判断这个邻居坐标在图像范围内，没有被访问过，同时状况是1
                        if 0 <= new[0] < L and 0 <= new[1] < M and 0 <= new[2] < N and three_d_image[new[0]][new[1]][
                            new[2]] == 1 and not visited[new[0]][new[1]][new[2]]:
                            visited[new[0]][new[1]][new[2]] = True
                            stk.append([new[0], new[1], new[2]])
                if count >= T:
                    ans += count
print(ans)
```

### C++

这里用`BFS` 广度优先遍历，算法和`Python`一样，但是少了很多节省行数的小技巧，所以会长很多。由于时间卡得很严，所以建议使用`C++` 来做此题。

```c++
#include<queue>
#include<iostream>

using namespace std;

int three_d_image[60][1286][128];
bool visited[60][1286][128];

struct point {
    int x, y, z;

    point(int x, int y, int z) : x(x), y(y), z(z) {};
};

int dx[6] = {1, 0, 0, -1, 0, 0};
int dy[6] = {0, 1, 0, 0, -1, 0};
int dz[6] = {0, 0, 1, 0, 0, -1};

int main() {
    // 数据读入
    int M, N, L, T;
    cin >> M >> N >> L >> T;
    for (int i = 0; i < L; i++) {
        for (int j = 0; j < M; j++) {
            for (int k = 0; k < N; k++) {
                cin >> three_d_image[i][j][k];
            }
        }
    }
    // 开始遍历
    int ans = 0;
    for (int i = 0; i < L; i++) {
        for (int j = 0; j < M; j++) {
            for (int k = 0; k < N; k++) {
                if (three_d_image[i][j][k] && !visited[i][j][k]) {
                    // 找到一个新块
                    int count = 0;
                    queue<point> que;
                    que.push(point(i, j, k));
                    visited[i][j][k] = true;
                    while (!que.empty()) {
                        struct point pnt = que.front();
                        que.pop();
                        count += 1;
                        // 遍历它的六个邻居
                        for (int p = 0; p < 6; p++) {
                            struct point new_point = point(pnt.x + dx[p], pnt.y + dy[p], pnt.z + dz[p]);
                            // 新点在坐标轴范围内，且没有被访问过，且点对应值是1
                                if (new_point.x >= 0 && new_point.y >= 0 && new_point.z >= 0 && new_point.x < L &&
                                new_point.y < M && new_point.z < N && !visited[new_point.x][new_point.y][new_point.z] &&
                                three_d_image[new_point.x][new_point.y][new_point.z]) {
                                que.push(new_point);
                                visited[new_point.x][new_point.y][new_point.z] = true;
                            }
                        }
                    }
                    if (count >= T)
                        ans += count;
                }
            }
        }
    }
    cout<<ans<<endl;
}
```

