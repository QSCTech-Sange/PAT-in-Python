---
title: <Python><PAT> 1110 Complete Binary Tree
date: 2019-09-04 11:03:08
tags:
- PAT
- 题解
- BFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 完全二叉树的广度优先遍历 
---

# 题目

Given a tree, you are supposed to tell if it is a complete binary tree.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤20) which is the total number of nodes in the tree -- and hence the nodes are numbered from 0 to *N*−1. Then *N* lines follow, each corresponds to a node, and gives the indices of the left and right children of the node. If the child does not exist, a `-` will be put at the position. Any pair of children are separated by a space.

### Output Specification:

For each case, print in one line `YES` and the index of the last node if the tree is a complete binary tree, or `NO` and the index of the root if not. There must be exactly one space separating the word and the number.

### Sample Input 1:

```in
9
7 8
- -
- -
- -
0 1
2 3
4 5
- -
- -
```

### Sample Output 1:

```out
YES 8
```

### Sample Input 2:

```in
8
- -
4 5
0 6
- -
2 3
- 7
- -
- -
```

### Sample Output 2:

```out
NO 1
```

# 题解

## 思路

+ 给定一棵树，让你判断其是否是完全二叉树。
+ 我们依照题目要求建树
+ 同时使用广度优先遍历，一层一层遍历
+ 因为完全二叉树除了最后一层，都是满的，所以可以通过层数对比数量来判断是否是完全二叉树
+ 但是最后一层得特殊处理一下，不仅仅要看数量，还要看位置
+ 必须是从左到右的顺序来填满。
+ 完全二叉树可以用树来表示，也可以用仿照堆的方法来表示，即用一个数组。
+ 但是使用数组的话，建立的顺序会有很大的困难，因此只留了更实用的树的表示方法。

## 数据结构

+ Node 是一个类，包含
  + index 下标
  + left 左孩子
  + right 右孩子
  + father 父节点
+ nodes 是一个列表，包含所有的Node
+ root 是这棵树的根
+ level 是BFS已经到了哪一层（从0开始计算）
+ max_level 是这棵树总共有几层
+ que 是一个队列
+ nodes_last 是最后一层剩余的节点数量
+ last_node 是最后一个节点的下标

## 算法

+ 首先根据题目的读取内容建立一棵树
+ 找到这棵树的树根
+ 从树根开始BFS，因为为了返回的便捷性，我使用了函数的方式，可以不使用。
+ 在bfs中
  + 首先计算出最大层数
  + 初始化第一层为0
  + 队列推入根节点
  + 设立一个永真循环
    + 在循环中，先判断队列已有的数量和这层的数量是否匹配
    + 不匹配直接返回No
    + level += 1,即开始统计下一层的节点数
    + 当这层不是最后一层时
      + 将队列里的所有节点的孩子节点添加到队列中
    + 当这层是最后一层时
      + 计算最后一层应该拥有几个节点
      + 初始化最后一个节点为0（这是因为如果树只有一个节点的话要输出最后一个节点就是0,不设这一步的话last_node就不会被赋值）
      + 对队列里的所有节点
      + 按每个节点的顺序，先看左边，再看右边
      + 当左边或右边有节点的时候相应剩余节点数量-1
      + 如果剩余节点不是0的话而这个节点的相应边是空的
      + 那么返回No
      + 否则，剩余节点为0 的时候，返回Yes

## 代码

+ 因为使用Python3能AC，因此只放了Python3的代码。

### Python 完全二叉树的树表示

```python
from queue import Queue


class Node:
    def __init__(self, val):
        self.index = val
        self.left = None
        self.right = None
        self.father = None


num_nodes = int(input())
nodes = [Node(i) for i in range(num_nodes)]
for i in range(num_nodes):
    left, right = input().split()
    if left != "-":
        nodes[i].left = nodes[int(left)]
        nodes[i].left.father = nodes[i]
    if right != "-":
        nodes[i].right = nodes[int(right)]
        nodes[i].right.father = nodes[i]

root = nodes[0]
while root.father is not None:
    root = root.father


def bfs():
    global num_nodes, root, num_nodes, nodes
    max_level = int(pow(num_nodes, 0.5))
    level = 0
    que = Queue()
    que.put(root)
    while True:
        if que.qsize() != pow(2, level):
            return "NO", root.index
        level += 1

        if level != max_level:
            for i in range(que.qsize()):
                node = que.get()
                if node.left is not None:
                    que.put(node.left)
                if node.right is not None:
                    que.put(node.right)

        else:
            nodes_last = num_nodes - pow(2, max_level) + 1
            last_node = 0
            for i in range(que.qsize()):
                node = que.get()
                if nodes_last != 0:
                    if node.left:
                        nodes_last -= 1
                        last_node = node.left.index
                    else:
                        return "NO", root.index
                if nodes_last != 0:
                    if node.right:
                        nodes_last -= 1
                        last_node = node.right.index
                    else:
                        return "NO", root.index
                if nodes_last == 0:
                    return "YES", last_node


a, b = bfs()
print(a, b)

```
