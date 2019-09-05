---
title: <Python><PAT> 1115 Counting Nodes in a BST
date: 2019-09-05 11:33:34
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 建立二叉搜索树，并层次遍历即可
---

# 题目

A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

- The left subtree of a node contains only nodes with keys less than or equal to the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

Insert a sequence of numbers into an initially empty binary search tree. Then you are supposed to count the total number of nodes in the lowest 2 levels of the resulting tree.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤1000) which is the size of the input sequence. Then given in the next line are the *N* integers in [−10001000] which are supposed to be inserted into an initially empty binary search tree.

### Output Specification:

For each case, print in one line the numbers of nodes in the lowest 2 levels of the resulting tree in the format:

```
n1 + n2 = n
```

where `n1` is the number of nodes in the lowest level, `n2` is that of the level above, and `n` is the sum.

### Sample Input:

```in
9
25 30 42 16 20 20 35 -5 28
```

### Sample Output:

```out
2 + 4 = 6
```

# 题解

## 思路

+ 建立二叉搜索树，按照给定的顺序。
  + 就是从根部开始找，小的往左走，大的往右走，找到空位插进去，很直观
+ 层次遍历，找到最深的两层即可。
+ 不难。

## 数据结构

+ Node 是一个类，包含三个属性
  + 值
  + 左孩子
  + 右孩子
+ root 是树根
+ ans 存放所有的层数的出现次数。比如[1,1,3,3,4,5]代表第一层出现了2次，第3层出现了2次，第四层出现了一次。
+ max_level 是最深的层数
+ a,b 是最深一层的节点个数和倒数第二层的节点个数。

## 算法

+ 插入算法，参数是节点和待插入的值
  + 先从根节点开始
  + 如果要插入的值小于等于这个节点，那么往左看
    + 如果左为空，插入它
    + 否则递归进入左节点，以左节点为根看能不能插入
  + 如果要插入的值大于这个节点，那么往左看
    - 如果右为空，插入它
    - 否则递归进入右节点，以右节点为根看能不能插入
+ 层次遍历。参数是节点和层级
  + 先把这个层级加入到ans里
  + 遍历左边的节点，层级 + 1
  + 遍历右边的节点，层级 + 1

## 代码

+ 这道题给的测试集并没有严格按照说好的方式来给，所以Python会有两个点非零返回。

### Python

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None


def insert(node, j):
    if j > node.val:
        if not node.right:
            node.right = Node(j)
        else:
            insert(node.right, j)
    else:
        if not node.left:
            node.left = Node(j)
        else:
            insert(node.left, j)


n = int(input())
nums = list(map(int, input().split()))
root = Node(nums[0])
for i in range(1,n):
	insert(root, j)

ans = []
def level(node, lev):
    ans.append(lev)
    if node.left:
        level(node.left, lev + 1)
    if node.right:
        level(node.right, lev + 1)


level(root,0)

max_level = max(ans)
a = ans.count(max_level)
b = ans.count(max_level - 1)
print("%d + %d = %d" % (a, b, a + b))

```

### C++

```c++
#include <iostream>
#include <vector>

using namespace std;

struct Node {
    int val;
    Node *left = nullptr;
    Node *right = nullptr;

    Node(int val) : val(val) {};
};

int insert(Node *node, int j) {
    if (j > node->val) {
        if (!node->right)
            node->right = new Node(j);
        else
            insert(node->right, j);
    } else {
        if (!node->left)
            node->left = new Node(j);
        else
            insert(node->left, j);
    }
}

vector<int> ans;

int level(Node *node, int lev){
    ans.push_back(lev);
    if (node->left)
        level(node->left, lev + 1);
    if (node->right)
        level(node->right, lev + 1);
}


int main() {
    int n;
    cin >> n;
    int nums[n];
    for (int i = 0; i < n; i++)
        cin>>nums[i];

    Node *root = new Node(nums[0]);
    for (int i= 1;i<n;i++)
        insert(root, nums[i]);

    level(root, 0);
    int max_level = 0;
    for (auto it:ans)
        max_level = max(max_level,it);
    int a = 0;
    int b = 0;
    for (auto it:ans){
        if (it == max_level)
            a += 1;
        else if (it == max_level - 1)
            b += 1;
    }
    printf("%d + %d = %d\n" ,a, b, a + b);
}
```

