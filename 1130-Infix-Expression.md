---
title: <Python><PAT> 1130 Infix Expression
date: 2019-09-05 16:13:08
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 稍微转换一下的中序遍历
---

# 题目

Given a syntax tree (binary), you are supposed to output the corresponding infix expression, with parentheses reflecting the precedences of the operators.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤ 20) which is the total number of nodes in the syntax tree. Then N lines follow, each gives the information of a node (the *i*-th line corresponds to the *i*-th node) in the format:

```
data left_child right_child
```

where `data` is a string of no more than 10 characters, `left_child` and `right_child` are the indices of this node's left and right children, respectively. The nodes are indexed from 1 to N. The NULL link is represented by −1. The figures 1 and 2 correspond to the samples 1 and 2, respectively.

| ![infix1.JPG](https://images.ptausercontent.com/4d1c4a98-33cc-45ff-820f-c548845681ba.JPG) | ![infix2.JPG](https://images.ptausercontent.com/b5a3c36e-91ad-494a-8853-b46e1e8b60cc.JPG) |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                           Figure 1                           |                           Figure 2                           |

### Output Specification:

For each case, print in a line the infix expression, with parentheses reflecting the precedences of the operators. Note that there must be no extra parentheses for the final expression, as is shown by the samples. There must be no space between any symbols.

### Sample Input 1:

```in
8
* 8 7
a -1 -1
* 4 1
+ 2 5
b -1 -1
d -1 -1
- -1 6
c -1 -1
```

### Sample Output 1:

```out
(a+b)*(c*(-d))
```

### Sample Input 2:

```in
8
2.35 -1 -1
* 6 1
- -1 4
% 7 8
+ 2 3
a -1 -1
str -1 -1
871 -1 -1
```

### Sample Output 2:

```out
(a*2.35)+(-(str%871))
```

# 题解

## 思路

+ 首先是建立二叉树
+ 然后就是中序遍历
+ 比较特殊的情况在于括号的处理
+ 如果这个点是叶子节点或者最外层节点，那么不用加括号
+ 否则，左右都要加括号。
+ 我们可以把答案先存储起来，然后移除最外层的括号
+ 要注意如果只有一个节点的特殊情况。

## 数据结构

+ Node 是一个节点，一个class，包含
  + val 值，也就是节点的内容，字符串
  + 左孩子
  + 右孩子
  + 父亲（为了便于找根）输出
+ nodes 存放所有的Node，可以方便用下标来索引
+ root 是这个树的树根
+ ans 存放答案

## 算法

+ 建立树
+ 找到树根
+ 中序遍历
  + 对非叶子节点的
    + 先放左括号
    + 递归左子树
    + 放自己
    + 递归右子树
    + 放右括号
  + 对叶子节点
    + 只放自己
+ 删掉左右括号输出

## 代码

+ 由于使用Python可以AC，因此只放了Python的题解。

```python
class Node:
    def __init__(self):
        self.val = None
        self.left = None
        self.right = None
        self.father = None


n = int(input())
nodes = [Node() for _ in range(n)]
for i in range(n):
    val, left, right = input().split()
    nodes[i].val = val
    if left != "-1":
        nodes[i].left = nodes[int(left) - 1]
        nodes[i].left.father = nodes[i]
    if right != "-1":
        nodes[i].right = nodes[int(right) - 1]
        nodes[i].right.father = nodes[i]

root = nodes[0]
while root.father:
    root = root.father

ans = []
def inorder(i: Node):
    if i.left or i.right:
        ans.append("(")
        if i.left:
            inorder(i.left)
        ans.append(i.val)
        if i.right:
            inorder(i.right)
        ans.append(")")
    else:
        ans.append(i.val)

inorder(root)
ans = ans[1:-1] if len(ans) > 1 else ans # 移除最外层括号，要注意只有一个节点的特殊情况。
print("".join(ans))
```

