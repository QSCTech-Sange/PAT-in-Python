---
title: <Python><PAT> 1138 Postorder Traversal
date: 2019-09-05 22:16:24
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 前序中序转后序
---

# 题目

Suppose that all the keys in a binary tree are distinct positive integers. Given the preorder and inorder traversal sequences, you are supposed to output the first number of the postorder traversal sequence of the corresponding binary tree.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤ 50,000), the total number of nodes in the binary tree. The second line gives the preorder sequence and the third line gives the inorder sequence. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in one line the first number of the postorder traversal sequence of the corresponding binary tree.

### Sample Input:

```in
7
1 2 3 4 5 6 7
2 3 1 5 4 7 6
```

### Sample Output:

```out
3
```

# 题解

## 思路

+ 题目给出了前序遍历和中序遍历

+ 求后序遍历的第一个数

+ 前序遍历是根左右

+ 中序遍历是左根右

+ 后序遍历是左右根

+ 我们可以这样画图来辅助理解，对于一棵树或子树，都符合下面的规律

+ ```
  前序：  根[    左    ][    右    ]
  中序：  [    左    ]根[    右    ]
  ```

+ 而我们想要的是后序遍历的第一项

+ 从逻辑上来说

  + 就是一个节点如果有左子树的，就深入左子树，不管右子树。
  + 如果有右子树的，就深入右子树。
  + 都没有的，输出它。

+ 这样子剪枝，就可以大大减少复杂度，将递归优化成尾递归

## 数据结构

+ pre 是前序序列
+ in 是中序序列
+ judge 是递归函数
+ pre_left 是在前序中的树的左边界
+ in_left,in_right 是在中序中的树的左右边界

## 算法

+ 照着思路来走一遍就可以了
+ 难点是树的左右边界坐标的确立，但是参考我前面画的图，就问题不大了。

##  代码

+ 由于这道题测试数据格式有误，Python有一个点会非零返回。

### Python 3

```python
def judge(pre_left, in_left, in_right):
    in_root = _in.index(pre[pre_left])
    if in_left <= in_root - 1:
        judge(pre_left + 1, in_left, in_root - 1)
    elif in_root + 1 <= in_right:
        judge(pre_left + (in_root - in_left) + 1, in_root + 1, in_right)
    else:
        print(_in[in_root])


n = int(input())
pre = list(map(int, input().split()))
_in = list(map(int, input().split()))
judge(0, 0, n - 1)

```
### C++
```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <set>

using namespace std;
void judge(const int pre[],const int in[],int pre_left,int in_left,int in_right){
    int in_root;
    for (int i = in_left;i<=in_right;++i){
        if (in[i] == pre[pre_left]){
            in_root = i;
            break;
        }
    }
    if (in_left <= in_root - 1)
        judge(pre,in,pre_left + 1, in_left, in_root - 1);
    else if(in_root + 1 <= in_right)
        judge(pre,in,pre_left + (in_root - in_left) + 1, in_root + 1, in_right);
    else
        cout<<in[in_root]<<endl;

}

int main() {
    int n;
    cin >> n;
    int pre[n];
    int in[n];
    for (int i = 0; i < n; i++)
        cin >> pre[i];
    for (int i = 0; i < n; i++)
        cin >> in[i];
    judge(pre,in,0,0,n-1);
}
```

