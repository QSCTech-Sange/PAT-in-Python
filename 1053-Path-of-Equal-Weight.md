---
title: <Python><PAT> 1053 Path of Equal Weight
date: 2019-08-27 16:29:00
tags: 
- PAT
- 题解
- DFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 普通的DFS加上排序的奇巧淫技
---

# 题目

Given a non-empty tree with root $R$, and with weight $W_i$ assigned to each tree node $T_i$. The **weight of a path from R to L** is defined to be the sum of the weights of all the nodes along the path from *R* to any leaf node *L*.

Now given any weighted tree, you are supposed to find all the paths with their weights equal to a given number. For example, let's consider the tree showed in the following figure: for each node, the upper number is the node ID which is a two-digit number, and the lower number is the weight of that node. Suppose that the given number is 24, then there exists 4 different paths which have the same given weight: {10 5 2 7}, {10 4 10}, {10 3 3 6 2} and {10 3 3 6 2}, which correspond to the red edges in the figure.

![img](https://images.ptausercontent.com/212)

### Input Specification:

Each input file contains one test case. Each case starts with a line containing $0<N \leq 100$, the number of nodes in a tree,$M(<N)$, the number of non-leaf nodes, and$0<S<2^{30}$, the given weight number. The next line contains $N$ positive numbers where $W_{i}(<1000)$ corresponds to the tree node $T_{i}$. Then *M* lines follow, each in the format:

```
ID K ID[1] ID[2] ... ID[K]
```

where `ID` is a two-digit number representing a given non-leaf node, `K` is the number of its children, followed by a sequence of two-digit `ID`'s of its children. For the sake of simplicity, let us fix the root ID to be `00`.

### Output Specification:

For each test case, print all the paths with weight S in **non-increasing** order. Each path occupies a line with printed weights from the root to the leaf in order. All the numbers must be separated by a space with no extra space at the end of the line.

Note: sequence $\left\{A_{1}, A_{2}, \cdots, A_{n}\right\}$ is said to be **greater than** sequence $\left\{B_{1}, B_{2}, \cdots, B_{m}\right\}$ if there exists $1 \leq k<\min \{n, m\}$ such that $A_{i}=B_{i}$ for $i=1, \cdots, k,$ and $A_{k+1}>B_{k+1}$.

### Sample Input:

```in
20 9 24
10 2 4 3 5 10 2 18 9 7 2 2 1 3 12 1 8 6 2 2
00 4 01 02 03 04
02 1 05
04 2 06 07
03 3 11 12 13
06 1 09
07 2 08 10
16 1 15
13 3 14 16 17
17 2 18 19
```

### Sample Output:

```out
10 5 2 7
10 4 10
10 3 3 6 2
10 3 3 6 2
```

### Special thanks to Zhang Yuan and Yang Han for their contribution to the judge's data.

# 题解

## 思路

+ 题目给的是一个节点有权重的无环图
+ 让你从根部找到符合相应权重的路径们
+ 注意，路径的起点是根部没有疑问，但是终点必须是叶子节点。这个可以从范例中看出来。
+ 那么很显然地就是DFS了。还不用记录有无访问过，比较简单的DFS
+ 但是对那些排序需要依靠一些小窍门
+ 题目的排序要求是，如果两个路径的第`i`个节点上相同，那么判断第`i+1`个节点，哪个小就是哪个路径小。
+ 一般来说我们容易写成`lambda x:(x[0],x[1],x[2],...)`这样的形式，可是问题是这些路径的长度是不确定的，而且不是所有路径的长度是一样的。
+ 我这里使用了一些奇巧淫技。
+ 注意这里的排序和字符串排序很像。
+ 那我们就可以把它映射成字符串的样子排序。
+ 注意不是直接用`str()` 这样映射，这样的话`10`反而比`9`小。
+ 要使用`chr()`这样排序，直接映射到ASCII码。
+ 这样就不需要单独列出来一个排序函数了。
+ 想到这点不容易（我夸我自己）

## 数据结构

+ weight 是一个列表，代表各个节点的权重
  + 下标是ID
  + 值是这个节点的权重
+ sons 是一个列表，代表各个节点的孩子节点
  + 下标是ID
  + 值是一个列表，包含这个ID的所有孩子
  + 如果没孩子则值为空
+ temp_weight 记录深搜过程中的临时权重和
+ path 记录神搜过程中的临时路径
+ ans记录答案，是一个列表，包含了所有符合要求的路径。
  + 一条路径是一个列表

## 算法

+ 如思路所说，深度优先遍历然后按照`chr()`组成的字符串排序。

## 代码

+ 因为使用Python能够AC，因此只放了Python的代码。

```python
# 深度优先遍历
def dfs(i):
    global temp_weight
    if temp_weight == target_weight and not sons[i]:　# 判断终止条件
        ans.append(path.copy())
        return
    elif temp_weight > target_weight:
        return

    if sons[i]:
        for son in sons[i]:
            temp_weight += weight[son]
            path.append(weight[son])
            dfs(son)
            temp_weight -= weight[son] #　以下两步是回溯
            path.pop()

# 数据结构初始化以及读入数据
num_nodes, num_non_leaf, target_weight = list(map(int, input().split()))
weight = list(map(int, input().split()))
sons = [None for _ in range(num_nodes)]
for _ in range(num_non_leaf):
    info = list(map(int, input().split()))
    sons[info[0]] = info[2:]

# 准备深度优先遍历
temp_weight = weight[0]
path = [weight[0]]
ans = []
dfs(0)

# 排序并输出
ans.sort(key=lambda x: "".join([chr(i) for i in x]),reverse=True)
for i in ans:
    print(" ".join(list(map(str, i))))

```

