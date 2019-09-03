---
title: <Python><PAT> 1106 Lowest Price in Supply Chain
date: 2019-09-03 18:55:43
tags:
- PAT
- 题解
- BFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 模拟传销层级，求最便宜的一环。这题对Python极不友好，建议使用C++
---

# 题目

A supply chain is a network of retailers（零售商）, distributors（经销商）, and suppliers（供应商）-- everyone involved in moving a product from supplier to customer.

Starting from one root supplier, everyone on the chain buys products from one's supplier in a price *P* and sell or distribute them in a price that is *r*% higher than *P*. Only the retailers will face the customers. It is assumed that each member in the supply chain has exactly one supplier except the root supplier, and there is no supply cycle.

Now given a supply chain, you are supposed to tell the lowest price a customer can expect from some retailers.

### Input Specification:

Each input file contains one test case. For each case, The first line contains three positive numbers: $N(\leq 10^5)$, the total number of the members in the supply chain (and hence their ID's are numbered from 0 to *N*−1, and the root supplier's ID is 0); *P*, the price given by the root supplier; and *r*, the percentage rate of price increment for each distributor or retailer. Then *N* lines follow, each describes a distributor or retailer in the following format:
$$
K_i ID[1] ID[2] ... ID[K_i]
$$


where in the *i*-th line, $K_i$ is the total number of distributors or retailers who receive products from supplier *i*, and is then followed by the ID's of these distributors or retailers. $K_j$ being 0 means that the *j*-th member is a retailer. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in one line the lowest price we can expect from some retailers, accurate up to 4 decimal places, and the number of retailers that sell at the lowest price. There must be one space between the two numbers. It is guaranteed that the all the prices will not exceed $10^{10}$.

### Sample Input:

```in
10 1.80 1.00
3 2 3 5
1 9
1 4
1 7
0
2 6 1
1 8
0
0
0
```

### Sample Output:

```out
1.8362 2
```

# 题解

## 思路

+ 模拟传销层级，找到最便宜的价格，那自然是用层级遍历，也就是用BFS啦！
+ 因为这样子我们是从最上层到最下层，当找到了一个层级有零售的，那么就可以直接跳出，用那一层的即可。
+ 注意如果只有一个节点，即个体户，这种情况要单独处理一下，因为min_level未被初始化。否则第二个点会过不了。

## 数据结构

+ next 是一个列表，记录下家
  + 下标是上家的id
  + 值是一个列表，包含这个上家的所有下家的id
+ level 是一个列表，记录层级
  + 下标是id
  + 值是层级。
  + 一开始只有根节点是第0层
+ found 记录有没有找到
+ min_level 是目前的最小层
+ num_min 是目前的最小层的
+ que 是一个队列，存放目前这一level的所有商户

## 算法

+ 根据输入更新next
+ 更新level，最初的是第0层
+ 预设min_num为1,min_level为无限大，found为false，开一个队列，放入根节点0
+ 当这个队列不为空的时候
  + 从中取出队伍首的商户
  + 遍历它的所有下家
  + 如果下家还有下家
    + 那么把这个下家添加进队列
  + 如果没有下家了
    + 那么这家就是价格最低的零售商
    + 更新min_level或min_num
  + 这时候队伍里的商户们可能还有下家一样是零售商
  + 所以还得继续遍历
  + 直到队伍里取出来的一个商户的level是刚刚的最小level，说明上一层已经遍历结束
+ BFS结束
+ 输出即可

## 代码

+ 这道题使用Python只能过三个点，建议使用C++。

### Python3

```python
from queue import Queue

n, price, rate = list(map(float, input().split()))
next = [[] for _ in range(int(n))]
level = [0 for _ in range(int(n))]
for i in range(int(n)):
    next[i] = list(map(int, input().split()[1:]))

# BFS
found = False
min_level = 999999
num_min = 1
que = Queue()
que.put(0)
while not que.empty():
    father = que.get()
    if found and level[father] == min_level:
        break
    for son in next[father]:
        level[son] = level[father] + 1
        if next[son]:
            que.put(son)
        else:
            found = True
            if min_level > level[son]:
                min_level = level[son]
            elif min_level == level[son]:
                num_min += 1
# 输出
if n != 1:
    print("%.4f %d" % (pow((1 + rate / 100), min_level) * price, num_min))
else:
    print("%.4f 1" % price)

```

### C++

```c++
#include <iostream>
#include <vector>
#include <queue>
#include <cmath>

using namespace std;

int main() {
    int n;
    double price, rate;
    cin >> n >> price >> rate;
    vector<int> next[n];
    int level[n];
    level[0] = 0;
    for (int i = 0; i < n; i++) {
        int num_son;
        scanf("%d", &num_son);
        for (int j = 0; j < num_son; j++) {
            int son;
            scanf("%d", &son);
            next[i].push_back(son);
        }
    }
    bool found = false;    // BFS
    int min_level = 999999;
    int num_min = 0;
    queue<int> que;
    que.push(0);
    while (!que.empty()) {
        int father = que.front();
        if (found && level[father] == min_level)
            break;
        que.pop();
        for (auto son:next[father]) {
            level[son] = level[father] + 1;
            if (!next[son].empty())
                que.push(son);
            else {
                found = true;
                if (min_level > level[son]) {
                    min_level = level[son];
                    num_min = 1;
                } else if (min_level == level[son])
                    num_min += 1;
            }
        }

    }
    if (n != 1)					// 输出
        printf("%.4f %d\n", pow((1 + rate / 100), min_level) * price, num_min);
    else
        printf("%.4f 1\n",price);
}

```

