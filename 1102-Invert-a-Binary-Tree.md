---
title: <Python><PAT> 1102 Invert a Binary Tree
date: 2019-09-03 13:41:29
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 二叉树的遍历
---

# 题目

The following is from Max Howell @twitter:

```
Google: 90% of our engineers use the software you wrote (Homebrew), but you can't invert a binary tree on a whiteboard so fuck off.
```

Now it's your turn to prove that YOU CAN invert a binary tree!

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤10) which is the total number of nodes in the tree -- and hence the nodes are numbered from 0 to *N*−1. Then *N* lines follow, each corresponds to a node from 0 to *N*−1, and gives the indices of the left and right children of the node. If the child does not exist, a `-` will be put at the position. Any pair of children are separated by a space.

### Output Specification:

For each test case, print in the first line the level-order, and then in the second line the in-order traversal sequences of the inverted tree. There must be exactly one space between any adjacent numbers, and no extra space at the end of the line.

### Sample Input:

```in
8
1 -
- -
0 -
2 7
- -
- -
5 -
4 6
```

### Sample Output:

```out
3 7 2 6 4 0 5 1
6 5 7 4 3 2 0 1
```

# 题解

## 思路

+ 题目里让我们建立一个左右相反的二叉树
+ 在输入的时候左右反一反就行了，水得很
+ 建立好树然后递归遍历就行了
+ 层次遍历要在递归中记录层数，按照“根左右”的顺序，其中左右按同一层算，最后按层数排序并输出
+ 中序遍历即“左根右”递归，并输出即可

## 数据结构

+ Node是一个class，是一个节点，成员有
  + val 值
  + left 左孩子
  + right 右孩子
  + father 父节点
+ root 根节点
+ ans 存放答案

## 算法

+ 首先建立一个列表，存放所有的节点（值就是下标）
+ 读入左右孩子的分布，这里反转一下左右
+ 利用father找到根部
+ 层次遍历并输出
+ 中序遍历并输出

## 代码

+ 由于使用Python能够AC，因此只使用了Python的代码。

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.father = None

# 数据结构初始化
n = int(input())
tree = [Node(i) for i in range(n)]
for i in range(n):
    right, left = input().split()
    if left != "-":
        tree[i].left = tree[int(left)]
        tree[i].left.father = tree[i]
    if right != "-":
        tree[i].right = tree[int(right)]
        tree[i].right.father = tree[i]
# 找到根部
root = tree[0]
while root.father is not None:
    root = root.father
# 存放答案
ans = []

# 层次遍历
def levelorder(node, level):
    ans.append([node.val, level])
    if node.left:
        levelorder(node.left, level + 1)
    if node.right:
        levelorder(node.right, level + 1)

levelorder(root, 0)
ans.sort(key=lambda x: x[1])
print(" ".join(list(map(str, [i[0] for i in ans]))))

# 中序遍历
def inorder(node):
    if node.left:
        inorder(node.left)
    ans.append(node.val)
    if node.right:
        inorder(node.right)

ans.clear()   
inorder(root)
print(" ".join(list(map(str, ans))))

```

