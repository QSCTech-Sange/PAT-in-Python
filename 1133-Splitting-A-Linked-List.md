---
title: <Python><PAT> 1133 Splitting A Linked List
date: 2019-09-05 18:36:47
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 奇奇怪怪的链表排序
---

# 题目

Given a singly linked list, you are supposed to rearrange its elements so that all the negative values appear before all of the non-negatives, and all the values in [0, K] appear before all those greater than K. The order of the elements inside each class must not be changed. For example, given the list being 18→7→-4→0→5→-6→10→11→-2 and K being 10, you must output -4→-6→-2→7→0→5→10→18→11.

### Input Specification:

Each input file contains one test case. For each case, the first line contains the address of the first node, a positive $N (≤10^5)$ which is the total number of nodes, and a positive $K (≤10^3)$. The address of a node is a 5-digit nonnegative integer, and NULL is represented by −1.

Then N lines follow, each describes a node in the format:

```
Address Data Next
```

where `Address` is the position of the node, `Data` is an integer in $[−10^5,10^5]$, and `Next` is the position of the next node. It is guaranteed that the list is not empty.

### Output Specification:

For each case, output in order (from beginning to the end of the list) the resulting linked list. Each node occupies a line, and is printed in the same format as in the input.

### Sample Input:

```in
00100 9 10
23333 10 27777
00000 0 99999
00100 18 12309
68237 -6 23333
33218 -4 00000
48652 -2 -1
99999 5 68237
27777 11 48652
12309 7 33218
```

### Sample Output:

```out
33218 -4 68237
68237 -6 48652
48652 -2 12309
12309 7 00000
00000 0 99999
99999 5 23333
23333 10 00100
00100 18 27777
27777 11 -1
```

# 题解

## 思路

+ 题目的意思是
+ 给了你一串链表
+ 然后让你把链表划分为三部分
  + 小于0的
  + 0到K之间的
  + 大于K的
+ 依次连接起来，其中每一部分保持原来的顺序不变
+ 我们设三个列表分表保存这三部分，输出的时候合并在一块，即可。

## 数据结构

+ K 即题目里的K
+ 三个部分的列表依次命名为
  + negative
  + zero_to_K
  + more_than_K
  + （很直观的命名吧！
+ _next 数组存放所有地址的下一个地址（原链表中的）
  + 下标是原地址
  + 值是它的下一个地址
+ val 数组存放所有地址的值
  + 下标是地址
  + 值是值（绕口令？
+ node 是遍历用节点
+ ans 将前面三个部分连接起来

## 算法

+ 首先读入数据，添加到_next和val数组当中。
+ 从起始节点开始遍历，并按情况添加到三个部分的列表。这样可以保证三个部分的正好也是按原来的顺序的。
+ 将三个部分拼接在一起。
+ 输出即可。

## 代码

+ 这道题使用Python会有一个点超时，建议使用C++

### Python

```python
addr_first, num, K = list(map(int, input().split()))
negative = []
zero_to_K = []
more_than_K = []
_next = [-1 for i in range(100001)]
val = [None for i in range(100001)]
# 读入数据
for _ in range(num):
    addr, value, next_addr = list(map(int, input().split()))
    val[addr] = value
    _next[addr] = next_addr
# 划分节点
node = addr_first
while node != -1:
    if val[node] < 0:
        negative.append(node)
    elif val[node] > K:
        more_than_K.append(node)
    else:
        zero_to_K.append(node)
    node = _next[node]
# 输出
ans = negative + zero_to_K + more_than_K
for i, j in enumerate(ans):
    if i != len(ans) - 1:
        print("%05d %d %05d" % (j, val[j], ans[i + 1]))
    else:
        print("%05d %d -1" % (j, val[j]))

```

### C++

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int addr_first,num,K;
    cin>>addr_first>>num>>K;
    vector<int> negative,zero_to_K,more_than_K;
    int _next[100001];
    int val[100001];
    // 读数据
    for (int i = 0;i <num;i++){
        int addr,value,next_addr;
        cin>>addr>>value>>next_addr;
        val[addr] = value;
        _next[addr] = next_addr;
    }
    // 划分节点
    int node = addr_first;
    while (node != -1){
        if (val[node] < 0)
            negative.push_back(node);
        else if (val[node] > K)
            more_than_K.push_back(node);
        else
            zero_to_K.push_back(node);
        node = _next[node];
    }
	// 输出
    vector<int> ans;
    ans.insert(ans.end(),negative.begin(),negative.end());
    ans.insert(ans.end(),zero_to_K.begin(),zero_to_K.end());
    ans.insert(ans.end(),more_than_K.begin(),more_than_K.end());
    for (int i = 0 ;i < ans.size() - 1;i++)
        printf("%05d %d %05d\n" , ans[i], val[ans[i]], ans[i + 1]);
    printf("%05d %d -1\n" ,ans.back(), val[ans.back()]);
}
```

