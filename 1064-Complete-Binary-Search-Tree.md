---
title: <Python><PAT> 1064 Complete Binary Search Tree
date: 2019-08-29 10:17:31
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 完全二叉树的层次遍历
---

# 题目

A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
- Both the left and right subtrees must also be binary search trees.

A Complete Binary Tree (CBT) is a tree that is completely filled, with the possible exception of the bottom level, which is filled from left to right.

Now given a sequence of distinct non-negative integer keys, a unique BST can be constructed if it is required that the tree must also be a CBT. You are supposed to output the level order traversal sequence of this BST.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤1000). Then *N* distinct non-negative integer keys are given in the next line. All the numbers in a line are separated by a space and are no greater than 2000.

### Output Specification:

For each test case, print in one line the level order traversal sequence of the corresponding complete binary search tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

### Sample Input:

```in
10
1 2 3 4 5 6 7 8 9 0
```

### Sample Output:

```out
6 3 8 1 5 7 9 0 2 4
```

# 题解

## 思路

+ 首先抓到题目的重点

+ 要将序列构造出一个完全BST

+ 那么BST的特点是什么呢?

+ 左小右大

+ 这样的特点就导致了一个遍历的特点

+ 就是中序的时候是从小到大的

+ 那么我们对原序列排序，即可得到中序遍历

+ 题目要求我们输出层次遍历

+ 所以题目意思变为中序转层序

+ 我们可以写一个递归完成这一点。

+ 用一个level记录层数，传递来的是一棵树的left和right，我们找到这棵数的根，然后递归进入左子树和右子树。用ans记录节点和层数。

+ ```python
  def count_level(left_begin, right_end, level):
      if left_begin > right_end:
          return
      root = # 还不知道
      ans.append([nodes[root], level])
      count_level(left_begin, root - 1, level + 1)
      count_level(root + 1, right_end, level + 1)
  ```

+ 因为我们总是先左后右，所以排完序同一层里也是先左后右。

+ 那么一开始传递的就是`count_level(0,len(nodes)-1,0)`

+ 最后再排序输出就可以了。

+ 问题是root该怎么找？

+ 如果二叉树是满二叉数，即最后一行是填满的，那么我们的中间值就是`(right_end + left_begin + 1) // 2`。

+ 但是问题是我们的左侧二叉树不一定是满的。

+ 所以我们计算这棵树最深到了哪一层，并假设这一层填满，这样采用同样的式子计算中间位置就OK了。

+ `right_end - left_begin + 1` 是这棵树的节点数量，记作`num_nodes`

+ 当节点数量为1的时候，就1层

+ 当节点数量在2到3的时候，2层

+ 当节点数量在4到7的时候，3层

+ 当节点数量在8到15的时候，4层

+ 所以我们的节点数量+1取2的对数的下顶，即是这棵树被填满的总层数，记作`filled_levels`

+ 而以2为底，以层数为指数，这样的`2^level - 1`就是这棵树被填满的总层数所容纳的总节点数，记作`filled_nodes`。

+ 回到我们的例子

+ ```
  									6
  						3							8
  			1					5			7				9
  		0		2			4			
  ```

+ 我们的root的索引如何找到？

+ 首先我们不算最后三个，先看被填满的部分。

+ ```
  				6
  		3					8
  	1		5			7		9	
  ```

+ 在这里，我们的根节点的索引应该是3

+ 就是`left_begin + (filled_nodes - 1) // 2`，左边的left_begin是基数，它要再加上所有数的一半，这个位置便是我们想要的位置。

+ 那么我们后面还剩`num_nodes - filled_nodes`，我们的root再加上它，即可。

+ 注意！这只是针对题目里的情况，如果`num_nodes - filled_nodes`超过了没填满的那一层的一半，这说明`num_nodes - filled_nodes`的一部分其实是在右子树当中的。于是更严谨地，我们再加上的应该是

+ `min(num_nodes - filled_nodes, (filled_nodes + 1) // 2)`后者是最后未填满的一层总容量的一半。

## 数据结构

+ nodes 保存原来的节点
+ ans是一个列表，保存我们的答案
  + 一个节点是一个列表，包含节点值和层数

## 算法

+ 都写在思路里了。

## 代码

+ 因为使用Python3 能AC，因此只放了Python3的代码。

```python
from math import floor, log2


def count_level(left_begin, right_end, level):
    if left_begin > right_end:
        return
    if left_begin == right_end:
        ans.append([nodes[left_begin], level])
        return
    num_nodes = right_end - left_begin + 1
    filled_nodes = pow(2, floor(log2(num_nodes))) - 1
    root = left_begin + (filled_nodes - 1) // 2 + min(num_nodes - filled_nodes, (filled_nodes + 1) // 2)
    ans.append([nodes[root], level])
    count_level(left_begin, root - 1, level + 1)
    count_level(root + 1, right_end, level + 1)


_ = input()
nodes = sorted(list(map(int, input().split())))
ans = []
count_level(0, len(nodes) - 1, 0)
ans.sort(key=lambda x: x[1])
print(" ".join(list(map(str, [i[0] for i in ans]))))

```

