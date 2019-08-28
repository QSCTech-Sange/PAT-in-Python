---
title: <Python><PAT> 1052 Linked List Sorting
date: 2019-08-27 15:32:48
tags: 
- PAT
- 题解
- 哈希表
- 链表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 模拟链表的排序
---

# 题目

A linked list consists of a series of structures, which are not necessarily adjacent in memory. We assume that each structure contains an integer `key` and a `Next` pointer to the next structure. Now given a linked list, you are supposed to sort the structures according to their key values in increasing order.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive *N* (<105) and an address of the head node, where *N* is the total number of nodes in memory and the address of a node is a 5-digit positive integer. NULL is represented by −1.

Then *N* lines follow, each describes a node in the format:

```
Address Key Next
```

where `Address` is the address of the node in memory, `Key` is an integer in [−105,105], and `Next` is the address of the next node. It is guaranteed that all the keys are distinct and there is no cycle in the linked list starting from the head node.

### Output Specification:

For each test case, the output format is the same as that of the input, where *N* is the total number of nodes in the list and all the nodes must be sorted order.

### Sample Input:

```in
5 00001
11111 100 -1
00001 0 22222
33333 100000 11111
12345 -1 33333
22222 1000 12345
```

### Sample Output:

```out
5 12345
12345 -1 00001
00001 0 11111
11111 100 22222
22222 1000 33333
33333 100000 -1
```

# 题解

## 思路

+ 看上去不难，但是坑太多了
+ 我一开始是这么想的，一个结点是数字和地址。
+ 我需要对链表排序，那么我就不需要一开始的每个节点的“下个节点地址”
+ 直接按一对儿存储所有数字和地址
+ 然后按照数字排序，再输出地址，数字，下一个的地址
+ 即便注意到了输出的地址格式必须是五位数，即便注意到了最后一个的地址是-1
+ 可是这样子却只过了两个点
+ 我甚至觉得这两点就是题目的全部的坑了
+ 但实际上是这样的
+ 题目所给的节点数据，并不全是有用的
+ 有些节点不在链表里，真正的有效节点顺着起点到了-1就没了。
+ 所以我们需要先把节点清洗一遍，保存在一个列表（vector）里
+ 还有一个坑就是，如果没有一个点是有效的，即vector为空，那要输出“0 -1”。即首地址是-1,为空的话就没法输出-1了。
+ 这两个坑没有意识到就没法AC了。

## 数据结构

+ next 是一个哈希表，表示原链表这个地址的下一个地址
  + 键是一个整形地址
  + 值是一个整形地址
+ values 是一个哈希表，表示这个地址上的值
  - 键是一个整形地址
  - 值是一个整数
+ clean_addr  是一个列表，存放那些有效的节点的地址。
+ 因为地址都是整数，所以哈希表可以用数组替代。使用哈希表显得优雅一点。

## 算法

+ 读取数据，更新next和values哈希表
+ 清理数据，从起始地址开始，将每个地址放到clean_addr列表里，直到地址是-1
+ 将clean_addr排序，排序依据是对应的values
+ 当clean_addr为空时，输出这种情况的解（特殊解）
+ 输出正常解
  + 先输出clean_addr的项数和第一项
  + 每行输出clean_addr 的下一项和它表示的值，以及clean_addr的再下一项
  + 当clean_addr到了最后一项的时候，不能遵照上面的规律，因为最后要输出-1

## 代码

### Python3

+ 这道题使用Python3会有一个点超时。建议使用C++。

```python
# 读取数据以及数据结构初始化
num_nodes, start_addr = list(map(int, input().split()))
values = dict()
next = dict()
for _ in range(num_nodes):
    addr, val, next_addr = list(map(int, input().split()))
    values[addr] = val
    next[addr] = next_addr

# 清理数据，找到符合要求的节点，并排序
clean_addr = []
addr = start_addr
while addr != -1:
    clean_addr.append(addr)
    addr = next[addr]
clean_addr.sort(key=lambda x: values[x])

# 输出
if len(clean_addr) == 0:		# 开头即0的特殊情况
    print("0 -1")
else:
    print("%d %05d" % (len(clean_addr), clean_addr[0]))
    for i in range(len(clean_addr) - 1):
        print("%05d %d %05d" % (clean_addr[i], values[clean_addr[i]], clean_addr[i + 1]))
    print("%05d %d -1" % (clean_addr[-1], values[clean_addr[-1]]))

```



### C++

+ 稳

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>

using namespace std;


unordered_map<int, int> values;

bool cmp(int a, int b) {
    return values[a] < values[b];
}

int main() {
    
    // 数据结构初始化与信息读入
    unordered_map<int, int> next;
    int num_nodes, start_addr;
    scanf("%d %d", &num_nodes, &start_addr);
    for (int i = 0; i < num_nodes; i++) {
        int addr, val, next_addr;
        scanf("%d %d %d", &addr, &val, &next_addr);
        values[addr] = val;
        next[addr] = next_addr;
    }
    
    // 清理数据并排序
    vector<int> clean_addr;
    int addr = start_addr;
    while (addr != -1) {
        clean_addr.push_back(addr);
        addr = next[addr];
    }
    sort(clean_addr.begin(), clean_addr.end(), cmp);
    
    // 输出答案
    if (clean_addr.size() == 0)
        printf("0 -1");
    else {
        printf("%d %05d\n", clean_addr.size(), clean_addr[0]);
        for (int i = 0; i < clean_addr.size() - 1; i++)
            printf("%05d %d %05d\n", clean_addr[i], values[clean_addr[i]], clean_addr[i + 1]);
        printf("%05d %d -1", clean_addr.back(), values[clean_addr.back()]);
    }
}
```

