---
title: <Python><PAT> 1020 Tree Traversals
date: 2019-08-22 09:24:43
tags: 
- PAT
- 题解
- 树
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 难度不小的树遍历
---

# 题目
Suppose that all the keys in a binary tree are distinct positive integers. Given the postorder and inorder traversal sequences, you are supposed to output the level order traversal sequence of the corresponding binary tree.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤30), the total number of nodes in the binary tree. The second line gives the postorder sequence and the third line gives the inorder sequence. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in one line the level order traversal sequence of the corresponding binary tree. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

```in
7
2 3 1 5 7 6 4
1 2 3 4 5 6 7
```

### Sample Output:

```out
4 1 6 3 5 7 2
```

# 题解

## 思路

+ 我感觉挺难的，借鉴了柳神的代码，但是修改了一些内容，更优雅
+ 我们先考虑前序输出，而不是层序
+ 首先，我们有一串左右根的输出和一串左根右的输出
+ 在后序里，最后一个一定是根。
+ 如果我们在中序里找到这个根，那么中序根左边就是左子树，右边就是右子树。
+ 左子树的根在哪里？因为后序遍历是左子树右子树根这样遍历，所以左子树的根就在后序遍历中的根部减去右子树的元素个数。而右子树的元素个数可以从中序遍历里找到
+ 右子树的根在哪里？同理，右子树的根就在后序遍历中根的左边第一个。
+ 这样子写递归，可以得到一个前序遍历，为了方便理解，就把变量名写得完整一些。

```python
def pre_order(root_index_post,tree_start_index_in,tree_end_index_in):
    if tree_start_index_in > tree_end_index_in:
        return
    root_index_in = in_order.index(post_order[root_index_post])
    print(post_order[root_index_post],end = ' ')
    pre_order(root_index_post - (tree_end_index_in-root_index_in+1),start,root_index_in-1)
    pre_order(root_index_post - 1,root_index_in + 1,end)
```

+ 调用的时候，`root_index_post`  就是 `num_nodes - 1`，而`tree_start_index_in`为 0 ，

`tree_end_index_in`为`num_nodes -1`

+ 如果转换为层次遍历呢？
+ 如果我们设进入pre_order的这个root处于x层，那么毫无疑问，在这个pre_order函数进入的左子树pre_order中，左子树的根的层数就是x+1层，相应的右子树也是。
+ 所以我们遍历的时候，可以在函数参数增加一个变量表示层数。
+ 同时先不记着打印，而是把(层数，值)作为一个元祖先存下来
+ 最后按照层数排序，把它们打印出来
+ 同一层的顺序会不会紊乱？不会的。因为我们遍历的时候，同一层也是先左后右的。（想象模拟一下先序遍历，根左右，永远先往左）
+ 那么问题就好办了！

## 数据结构

+ num_nodes 节点个数
+ post 后序遍历序列
+ inorder 前序遍历序列
+ ans 答案数组，保存元祖，每一个元祖代表一个节点
  + 元祖第一项代表层数
  + 第二项代表值
+ root_post 为后序遍历的根
+ start_in 为这次递归的树中的子树起点（中序）
+ end_in 为这次递归的树中的子树终点（中序）
+ level 为层数

## 算法

思路里其实都讲清楚了

## 代码

因为使用Python能AC，因此只写了Python的代码。

虽然不到15行但是是真的难。

```python
num_nodes = int(input())
post = list(map(int, input().split()))  # 左右根
inorder = list(map(int, input().split()))  # 根左右
ans = []

# 递归函数
def pre(root_post, start_in, end_in, level):
    if start_in > end_in:
        return
    root_in = inorder.index(post[root_post])  # 中序遍历中的根索引，从后序里找
    ans.append((level, post[root_post]))
    pre(root_post - 1 - end_in + root_in, start_in, root_in - 1, level + 1)
    pre(root_post - 1, root_in + 1, end_in, level + 1)

# 开始递归，排序并输出
pre(num_nodes - 1, 0, num_nodes - 1, 0)
ans.sort(key=lambda x: x[0])
print(" ".join(list(map(lambda x: str(x[1]), ans))))

```

