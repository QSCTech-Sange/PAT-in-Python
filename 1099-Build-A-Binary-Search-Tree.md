---
title: <Python><PAT> 1099 Build A Binary Search Tree
date: 2019-09-03 10:39:58
tags: 
- PAT
- 题解
- BST
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 根据前序建立BST，并输出层序
---

# 题目

A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
- Both the left and right subtrees must also be binary search trees.

Given the structure of a binary tree and a sequence of distinct integer keys, there is only one way to fill these keys into the tree so that the resulting tree satisfies the definition of a BST. You are supposed to output the level order traversal sequence of that tree. The sample is illustrated by Figure 1 and 2.

![BST](https://images.ptausercontent.com/24c2521f-aaed-4ef4-bac8-3ff562d80a1b.jpg)

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤100) which is the total number of nodes in the tree. The next *N* lines each contains the left and the right children of a node in the format `left_index right_index`, provided that the nodes are numbered from 0 to *N*−1, and 0 is always the root. If one child is missing, then −1 will represent the NULL child pointer. Finally *N* distinct integer keys are given in the last line.

### Output Specification:

For each test case, print in one line the level order traversal sequence of that tree. All the numbers must be separated by a space, with no extra space at the end of the line.

### Sample Input:

```in
9
1 6
2 3
-1 -1
-1 4
5 -1
-1 -1
7 -1
-1 8
-1 -1
73 45 11 58 82 25 67 38 42
```

### Sample Output:

```out
58 25 82 11 38 67 45 73 42
```

# 题解

## 思路

+ 题目的中心思想就是根据给定的树形状和值，建立一个树，并输出它的层次遍历。
+ 题目里每棵节点都是有下标的，那么我们可以先建一个列表来放置所有的节点。下标与题目含义一样
+ 然后根据读进来的每个节点的左右孩子，先更新这棵树的形状
+ 接着读取所有的树的值
+ 我们将这些值排序，其实就是这棵数的中序遍历（即左根右，根据BST的性质就是从小到大）
+ 那么我们可以写一个递归，来给这棵数赋值，这个递归就模拟中序遍历。
  + 这个递归函数读取一个节点
    + 如果这个节点的左侧还有子树，那么递归进入左子树
    + 给这个节点赋值为排序后的值的第一项，同时删除排序后的列表的第一项
    + 如果这个节点的右侧还有子树，那么递归进入右子树
+ 一开始调用递归的节点即根节点nodes[0]
+ 然后开始层次遍历
+ 我们新开一个ans保存答案
+ 层次遍历的时候，不仅要记录节点，还要记录层数
  + 先将遍历着的节点的层数和值添加进ans里
  + 如果这个节点的左侧还有子树，那么递归进入左子树，同时层数 + 1
  + 如果这个节点的左侧还有子树，那么递归进入右子树，同时层数 + 1
+ 一开始调用递归的节点即根节点nodes[0]
+ 对ans按照层数排序
+ 输出ans的每一项的值即可
+ 因为是先进入左子树再进入右子树的，所以对于ans来说，相同层数的永远是左边在先，这样符合我们层次遍历的意义。

## 数据结构

+ Node是一个节点，是一个class，包含
  + val 值
  + left 左孩子
  + right 右孩子
+ nodes 是一个列表， 包含所有的node。
  + 下标即题目里的node下标
  + 那么根就是nodes[0]
+ nums 是一个列表，包含所有节点的值
+ ans 是我们要输出的答案，是一个列表
  + 每一项表示一个节点，也是一个列表
    + 第一项表示节点的值
    + 第二项表示节点的层次

## 算法

+ 都写在思路里了。

## 代码

+ 由于使用Python可以AC，因此只放了Python的解。

```python
class Node:
    def __init__(self):
        self.val = None
        self.left = None
        self.right = None

num_nodes = int(input())
nodes = [Node() for _ in range(num_nodes)]
for i in range(num_nodes):
    left, right = list(map(int, input().split()))
    nodes[i].left = nodes[left] if left != -1 else None
    nodes[i].right = nodes[right] if right != -1 else None

nums = sorted(list(map(int, input().split())))

# 中序遍历给树赋值
def val(node):
    if node.left:
        val(node.left)
    node.val = nums[0]
    del nums[0]
    if node.right:
        val(node.right)

val(nodes[0])

ans = []
# 层次遍历
def level_order(node, level):
    ans.append([node.val, level])
    if node.left:
        level_order(node.left, level + 1)
    if node.right:
        level_order(node.right, level + 1)

level_order(nodes[0], 0)
ans.sort(key=lambda x: x[1])
print(" ".join(list(map(str, [i[0] for i in ans]))))
```

