---
title: <Python><PAT> 1086 Tree Traversals Again
date: 2019-09-02 16:21:50
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 迷惑树遍历
---

# 题目

An inorder binary tree traversal can be implemented in a non-recursive way with a stack. For example, suppose that when a 6-node binary tree (with the keys numbered from 1 to 6) is traversed, the stack operations are: push(1); push(2); push(3); pop(); pop(); push(4); pop(); pop(); push(5); push(6); pop(); pop(). Then a unique binary tree (shown in Figure 1) can be generated from this sequence of operations. Your task is to give the postorder traversal sequence of this tree.

![Figure 1](https://images.ptausercontent.com/30)


### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤30) which is the total number of nodes in a tree (and hence the nodes are numbered from 1 to *N*). Then 2*N* lines follow, each describes a stack operation in the format: "Push X" where X is the index of the node being pushed onto the stack; or "Pop" meaning to pop one node from the stack.

### Output Specification:

For each test case, print the postorder traversal sequence of the corresponding tree in one line. A solution is guaranteed to exist. All the numbers must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

```in
6
Push 1
Push 2
Push 3
Pop
Pop
Push 4
Pop
Pop
Push 5
Push 6
Pop
Pop
```

### Sample Output:

```out
3 4 2 6 5 1
```

# 题解

## 思路

+ 有两种思想
+ 一种是根据栈建树，然后后序遍历，比较直接
+ 另一种是根据push顺序是前序遍历，pop顺序是中序遍历，然后根据这两种遍历来求出后序遍历，不用建树
+ 以前我都用第二种来写题解，这次用第一种
+ 第二种比较有技巧性和抽象，第一种更直观
+ 但是这题的根据栈建树有一点特别迷惑
+ 我基本思路是这样的，push的话
+ 检查父节点（栈顶）的左侧是否为空，空的话就在左侧插入
+ 否则就在右侧插入
+ pop的话直接把栈顶的弹出
+ 但是实际上并不是这样的
+ 这个pop操作非常的迷惑
+ 当你pop了之后，你的父节点是pop出来的节点，而不是新的栈顶
+ 这点琢磨了好久，从样例里琢磨出来的，并不清楚为什么要搞这样，太抽象了。想不到真的完蛋。
+ 所以每次pop的时候，记录father是弹出来的节点。
+ 而每次push的时候，如果上一次是pop，用pop出来的father。否则，用栈顶。
+ 后序遍历用递归即可。因为不能末尾有多余的空格，因此只能用一个列表先把答案保存一下。

## 数据结构

+ Node 是一个节点，是一个class
  + 包含左
  + 右
  + 和节点的值
  + 我以前都是用列表表示的，因为省行数，不过不得不承认使用class的话确实会清晰很多，也是更多人常用的选择吧。所以这次用了class
+ stack 是栈，推入的是Node
+ ans 存放答案
+ Father 是父节点，一开始自然为空

## 算法

+ 初始化数据结构
+ 遍历每个操作
+ 当操作是pop的时候
  + 记录father是栈顶
  + 栈弹出
+ 当操作是push的时候
  + 如果father不存在，说明是第一次添加，记录树根
  + 找father左边有没有空位
  + 有就往左边插
  + 没有就往右边插
  + 同时更新栈
  + 更新father为栈顶
+ 后序遍历
+ 利用一个递归，先遍历左，再遍历右，再记录自己，就OK了
+ 输出

## 代码

+ 由于使用Python能够AC，因此只放了Python的代码。

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None


num = int(input())
stack = []
father = None
for i in range(2 * num):
    info = input()
    if info == "Pop":
        father = stack[-1]
        stack.pop()
    else:
        if not father:
            stack.append(Node(int(info.split()[1])))
            root = stack[0]
        elif not father.left:
            stack.append(Node(int(info.split()[1])))
            father.left = stack[-1]
        else:
            stack.append(Node(int(info.split()[1])))
            father.right = stack[-1]
        father = stack[-1]


def post(node):
    if node.left:
        post(node.left)
    if node.right:
        post(node.right)
    ans.append(node.val)


ans = []
post(root)
print(" ".join(list(map(str, ans))))
```

