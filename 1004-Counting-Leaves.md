---
title: <Python><PAT> 1004 Counting Leaves
date: 2019-08-19 14:07:20
tags: 
- PAT
- 题解
- BFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 比较简单的广度优先遍历
---

# 题目

A family hierarchy is usually presented by a pedigree tree. Your job is to count those family members who have no child.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 0<*N*<100, the number of nodes in a tree, and *M* (<*N*), the number of non-leaf nodes. Then *M* lines follow, each in the format:

```
ID K ID[1] ID[2] ... ID[K]
```

where `ID` is a two-digit number representing a given non-leaf node, `K` is the number of its children, followed by a sequence of two-digit `ID`'s of its children. For the sake of simplicity, let us fix the root ID to be `01`.

The input ends with *N* being 0. That case must NOT be processed.

### Output Specification:

For each test case, you are supposed to count those family members who have no child **for every seniority level** starting from the root. The numbers must be printed in a line, separated by a space, and there must be no extra space at the end of each line.

The sample case represents a tree with only 2 nodes, where `01` is the root and `02` is its only child. Hence on the root `01` level, there is `0` leaf node; and on the next level, there is `1` leaf node. Then we should output `0 1` in a line.

### Sample Input:

```in
2 1
01 1 02
```

### Sample Output:

```out
0 1
```

# 题解

+ 首先理解题意
+ 题目的意思是给你一棵树，让你输出每层的叶子节点个数
+ 那很明显就是广度优先遍历了

## 数据结构

+ members 列表记录所有成员
  + 下标是成员id
  + 值是一个列表，包含这个成员的所有孩子的id
+ level 列表记录每层的叶子节点个数
  + 下标是层数
  + 值是叶子节点个数
+ temp_num是计算这层的叶子结点的中间变量

## 算法

+ 详细讲讲广度优先遍历的内容
+ 广度优先遍历要配合先入先出的队列，队列初始化为拥有一个根节点。队列存放的所有节点都拥有相同的层数。
+ 当队列不为空的时候
  + 记录现在队列的长度
  + temp_num 清零
  + 遍历现在这个队列
    + 遍历队列里每一项的所有孩子
      + 如果孩子是没有孩子的，temp_num + 1
      + 否则，添加到队列中
  + level添加temp_num项
+ 最后输出level即可
+ 注意只有一个节点的特殊情况！

## 代码

因为Python3 能AC，因此使用Python3的代码，只有寥寥20行，我爽爆。

```python
# 读入第一行信息
num_nodes, num_non_leaf = list(map(int, input().split()))
# 先把只有一个根节点的特殊情况给处理咯
if num_nodes == 1:print(1)
else:
    # 初始化数据结构
    members = [[] for _ in range(num_nodes + 1)]
    for _ in range(num_non_leaf):
        a = list(map(int, input().split()))
        members[a[0]] = a[2:]
    # 广度优先遍历
    level,que = [0],[1]
    while que:
        length,temp_num = len(que),0
        for _ in range(length):
            father = que.pop()
            for son in members[father]:
                if not members[son]:temp_num += 1
                else:que.insert(0, son)
        level.append(temp_num)
    # 输出结果
    print(" ".join(list(map(str, level))))
```

