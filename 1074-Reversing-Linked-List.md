---
title: <Python><PAT> 1074 Reversing Linked List
date: 2019-08-31 15:26:13
tags: 
- PAT
- 题解
- 链表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 链表逆序
---

# 题目

Given a constant *K* and a singly linked list *L*, you are supposed to reverse the links of every *K* elements on *L*. For example, given *L* being 1→2→3→4→5→6, if *K*=3, then you must output 3→2→1→6→5→4; if *K*=4, you must output 4→3→2→1→5→6.

### Input Specification:

Each input file contains one test case. For each case, the first line contains the address of the first node, a positive *N* (≤105) which is the total number of nodes, and a positive *K* (≤*N*) which is the length of the sublist to be reversed. The address of a node is a 5-digit nonnegative integer, and NULL is represented by -1.

Then *N* lines follow, each describes a node in the format:

```
Address Data Next
```

where `Address` is the position of the node, `Data` is an integer, and `Next` is the position of the next node.

### Output Specification:

For each case, output the resulting ordered linked list. Each node occupies a line, and is printed in the same format as in the input.

### Sample Input:

```in
00100 6 4
00000 4 99999
00100 1 12309
68237 6 -1
33218 3 00000
99999 5 68237
12309 2 33218
```

### Sample Output:

```out
00000 4 33218
33218 3 12309
12309 2 00100
00100 1 99999
99999 5 68237
68237 6 -1
```

# 题解

## 思路

+ 首先理解题意
+ 题目给你一个链表，和一个数字
+ 这个数字K是什么意思呢
+ 从第1个到第K个的链表节点，要反转
+ 然后从第K+1 到 2K的链表节点，要反转
+ 就这么反转下去
+ 我们存储链表优雅一点可以使用哈希表，暴力一点可以使用数组
+ 既然空间足够，那就开数组，反而会快一点。
+ 即开一个next数组和一个num数组。
  + 下标对应地址
  + next的值对应下一个节点的地址
  + num的值对应这个地址上的数
+ 对哈希表来说，开一个字典
  + 下标对应地址
  + 值对应一个列表，第一项是数值，第二项是下个节点的地址
+ 我们要首先遍历到第K个节点，然后从第K个开始一个一个往前驱节点连。
+ 这里可以在保存节点的时候记录每个节点的前驱节点，也可以每次遍历记录前K个数，放在一个数组里。
+ 我这里选择了第二种，两种方法都是OK的，完全看你的喜好。
+ 之后就是每到第NK个节点，把(N-1)K + 1到NK的节点进行逆序。
+ 但是！要注意边界条件！这才是这道题最棘手的地方。
+ 我这里使用了Count来记录读入的节点数量，当count%K == 0的时候，代表读到了第K个节点，要开始反转了。
+ 每次要对前K个节点反转的时候，要记下来第K+1个节点！同时把新的第K个节点——也就是第0个节点——的尾巴连到之前记下来的K+1节点。(1)
+ 而当count==K的时候，便是第一次反转。
+ 第一次反转要注意的是，更新start_addr，也就是起始地址，为第K个节点的地址。
+ 如果不是第一次反转的话，也要注意！
+ 上一次反转成功后，原来的第K个节点连接到的是原来的第K+1节点。
+ 举例来说，123456，按照长度为3的K，第一次反转后，变成321456，这时候1连接的是4（根据标记点(1)所述的方式）。
+ 而我们第二次反转要变成321654
+ 可是此时1连接的还是4,我们想要1连接的是6
+ 所以我们每次逆序完，要记录一个last_addr，也就是新的第K个节点。
+ 这样下一次遍历完，就可以把新的第K个节点连接到新的第K+1个节点。
+ 注意，在(1)说的那一步是必不可少的，也就是将新的第K个节点连接到旧的第K+1个节点是必要的。因为你不知道K+1后面的节点能不能有K个，够不够组成反转的一轮。

## 数据结构

+ start_addr 起始地址
+ nodes_num 节点数量
+ K 题目里的反转基数
+ 在哈希表算法中
  + nodes 是记录所有节点的字典
    + 下标对应地址
    + 值对应一个列表，第一项是数值，第二项是下个节点的地址
+ 在数组算法中
  + next数组
    - 下标对应地址
    - next的值对应下一个节点的地址
  + num数组
    + 下标对应地址
    + num的值对应这个地址上的数
+ temp_nodes 记录每次的K个节点的地址
+ addr 是用来遍历的地址
+ count 记录遍历到了第几个节点
+ last_addr 记录了一次反转完成后的K个地址中的那第K个节点的地址

## 算法

+ 参照思路，讲得很清楚了。

## 代码

+ Python的所有解都会有一个点超时。这里放了哈希表和数组的Python解，和数组的C++解。C++解可以AC。当然你照着真实的链表建也不是不可以，就是怪麻烦的。

### Python 哈希表
```python
# 读入信息与数据结构初始化
start_addr, nodes_num, K = list(map(int, input().split()))
nodes = dict()
for _ in range(int(nodes_num)):
    this, num, next = list(map(int, input().split()))
    nodes[this] = [num, next]
# 输入
temp_nodes = [start_addr]
addr = start_addr
count = 1
while addr != -1:
    if count % K != 0:
        addr = nodes[addr][1]
        temp_nodes.append(addr)
        count += 1
    else:
        addr = nodes[addr][1]
        for i in range(len(temp_nodes) - 1, 0, -1):
            nodes[temp_nodes[i]][1] = temp_nodes[i - 1]
        if count == K:
            start_addr = temp_nodes[K-1]
        else:
            nodes[last_addr][1] = temp_nodes[K - 1]
        last_addr = temp_nodes[0]
        nodes[temp_nodes[0]][1] = addr
        temp_nodes = [addr]
        count += 1
# 输出
addr = start_addr
while addr != -1:
    if nodes[addr][1] == -1:
        print("%05d %d -1" % (addr, nodes[addr][0]))
    else:
        print("%05d %d %05d" % (addr, nodes[addr][0], nodes[addr][1]))
    addr = nodes[addr][1]
```
### Python 数组
```python
# 读入信息与数据结构初始化
start_addr, nodes_num, K = list(map(int, input().split()))
next = [None for _ in range(100000)]
num = [None for _ in range(100000)]
for _ in range(int(nodes_num)):
    this, nu, nex = list(map(int, input().split()))
    next[this] = nex
    num[this] = nu
# 反转
temp_nodes = [start_addr]
addr = start_addr
count = 1
while addr != -1:
    if count % K != 0:
        addr = next[addr]
        temp_nodes.append(addr)
        count += 1
    else:
        addr = next[addr]
        for i in range(len(temp_nodes) - 1, 0, -1):
            next[temp_nodes[i]] = temp_nodes[i - 1]
        if count == K:
            start_addr = temp_nodes[K-1]
        else:
            next[last_addr] = temp_nodes[K - 1]
        last_addr = temp_nodes[0]
        next[temp_nodes[0]] = addr
        temp_nodes = [addr]
        count += 1
# 输出
addr = start_addr
while addr != -1:
    if next[addr] == -1:
        print("%05d %d -1" % (addr, num[addr]))
    else:
        print("%05d %d %05d" % (addr, num[addr], next[addr]))
    addr = next[addr]

```
### C++ 数组
```c++
#include <iostream>

using namespace std;

int main() {
    // 读入信息与数据结构初始化
    int start_addr, nodes_num, K;
    scanf("%d %d %d", &start_addr, &nodes_num, &K);
    int next[100000];
    int num[100000];
    for (int i = 0; i < nodes_num; i++) {
        int a, b, c;
        scanf("%d %d %d", &a, &b, &c);
        next[a] = c;
        num[a] = b;
    }
    // 反转
    int temp_nodes[K];
    temp_nodes[0] = start_addr;
    int addr = start_addr;
    int count = 1;
    int last_addr = -1;
    while (addr != -1) {
        if (count % K != 0) {
            addr = next[addr];
            temp_nodes[count % K] = addr;
            count += 1;
        } else {
            addr = next[addr];
            for (int i = K - 1; i >= 0; i--)
                next[temp_nodes[i]] = temp_nodes[i - 1];
            if (count == K)
                start_addr = temp_nodes[K - 1];
            else
                next[last_addr] = temp_nodes[K - 1];
            last_addr = temp_nodes[0];
            next[temp_nodes[0]] = addr;
            temp_nodes[0] = addr;
            count += 1;
        }

    }
    // 输出
    addr = start_addr;
    while (addr != -1) {
        if (next[addr] == -1)
            printf("%05d %d -1\n", addr, num[addr]);
        else
            printf("%05d %d %05d\n", addr, num[addr], next[addr]);
        addr = next[addr];
    }
}
```

