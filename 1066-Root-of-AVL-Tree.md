---
title: <Python><PAT> 1066 Root of AVL Tree
date: 2019-08-29 15:21:31
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 应该是超纲题。
---

# 题目

An AVL tree is a self-balancing binary search tree. In an AVL tree, the heights of the two child subtrees of any node differ by at most one; if at any time they differ by more than one, rebalancing is done to restore this property. Figures 1-4 illustrate the rotation rules.



![img](https://images.ptausercontent.com/31) ![img](https://images.ptausercontent.com/32)



![img](https://images.ptausercontent.com/33) ![img](https://images.ptausercontent.com/34)

Now given a sequence of insertions, you are supposed to tell the root of the resulting AVL tree.



### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤20) which is the total number of keys to be inserted. Then *N* distinct integer keys are given in the next line. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the root of the resulting AVL tree in one line.

### Sample Input 1:

```in
5
88 70 61 96 120
```

### Sample Output 1:

```out
70
```

### Sample Input 2:

```
7
88 70 61 96 120 90 65
```

### Sample Output 2:

```
88
```

# 题解

## 思路

+ 考纲里没有AVL树的内容，所以应该是不考的。
+ 写法就是按照题目意思，构造一个AVL BST。

## 数据结构

+  Node 是一个节点，属性包含
  + val
  + left
  + right

## 算法

+ rotateLeft 是左旋
  + 就是当右边太长的时候
  + 把右节点当成根节点
  + 方法是，根的右节点改为右节点的左节点，而右节点的左节点改为根。
  + 返回这个新的根节点
+ rotateRight 是右旋
  + 就是当左边太长的时候
  + 把左节点当成根节点
  + 方法是，原根的左节点改为左节点的右节点，而左节点的右节点改为原根。
  + 返回这个新的根节点
+ 以上两步画图就好理解了。
+ getHeight 找到这个节点的高度，这个没什么问题
+ insert 插入一个数，并返回根节点。
  + 插入的时候，从根节点不断比大小找到应该放的位置
  + 放完了后，判断有没有造成不平衡
  + 即左子树和右子树的差的绝对值有没有大于2
  + 不平衡了那么就需要rotate
  + 但是要注意一件事情
  + 举例来说，如果我往左侧插入一个数，造成了不平衡，左边太长了，这时候想到要右旋
  + 如果要插入的数比根结点的左侧还要大，即，它应该才是根的左节点
  + 这种情况下，要先对左子树进行左旋。
  + 为什么要这样做呢？因为每次右旋都会使根变成根的左节点，而这时候根的左节点并不是新节点，所以右旋是错误的。
  + 我们这样处理，就相当于把原来的根节点的左节点压下去了。
+ 反正是超纲的，问题不大，不会不用管就行。

## 代码

+ 因为使用Python3能AC，所以只放了Python的代码。

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

    def rotateLeft(self):
        node = self.right
        self.right = node.left
        node.left = self
        return node

    def rotateRight(self):
        node = self.left
        self.left = node.right
        node.right = self
        return node

    def getHeight(self):
        left = 0 if not self.left else self.left.getHeight()
        right = 0 if not self.right else self.right.getHeight()
        return max(left, right) + 1

    def insert(self, val):
        if not self.val:
            self.val = val
            return self

        if self.val > val:
            self.left = Node(val) if not self.left else self.left.insert(val)
            left = 0 if not self.left else self.left.getHeight()
            right = 0 if not self.right else self.right.getHeight()
            if (left - right) == 2:
                if val >= self.left.val:
                    self.left = self.left.rotateLeft()
                self = self.rotateRight()
        else:
            self.right = Node(val) if not self.right else self.right.insert(val)
            left = 0 if not self.left else self.left.getHeight()
            right = 0 if not self.right else self.right.getHeight()
            if (left - right) == -2:
                if val <= self.right.val:
                    self.right = self.right.rotateRight()
                self = self.rotateLeft()
        return self


root = Node(None)
_ = input()
for i in input().split():
    root = root.insert(int(i))
print(root.val)

```

