---
title: <Python><PAT> 1097 Deduplication on a Linked List
date: 2019-08-18 11:08:58
tags: 
- PAT
- 题解
- 链表
- 哈希表
- 哈希集合
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: C++ 暴力解都能过，Python 优雅解都超时两个点，无语。
---

# 题目

Given a singly linked list *L* with integer keys, you are supposed to remove the nodes with duplicated absolute values of the keys. That is, for each value *K*, only the first node of which the value or absolute value of its key equals *K* will be kept. At the mean time, all the removed nodes must be kept in a separate list. For example, given *L* being 21→-15→-15→-7→15, you must output 21→-15→-7, and the removed list -15→15.

### Input Specification:

Each input file contains one test case. For each case, the first line contains the address of the first node, and a positive *N* (≤105) which is the total number of nodes. The address of a node is a 5-digit nonnegative integer, and NULL is represented by −1.

Then *N* lines follow, each describes a node in the format:

```
Address Key Next
```

where `Address` is the position of the node, `Key` is an integer of which absolute value is no more than 104, and `Next` is the position of the next node.

### Output Specification:

For each case, output the resulting linked list first, then the removed list. Each node occupies a line, and is printed in the same format as in the input.

### Sample Input:

```in
00100 5
99999 -7 87654
23854 -15 00000
87654 15 -1
00000 -15 99999
00100 21 23854
```

### Sample Output:

```out
00100 21 23854
23854 -15 99999
99999 -7 -1
00000 -15 87654
87654 15 -1
```

# 思路

这道题时间设置得很不好， C++ 暴力解都能过，Python 优雅解都超时两个点，无语。完全无法考验考生的代码能力。题目想要你在**原地**修改链表。为了让大家学习算法，两种思路我都cover了。

## 优雅解

+ 使用哈希表将地址映射到节点
+ 每一个节点包含数字和下一个节点的地址
+ 开一个哈希集合，存放已经有的数字。
+ 开一个record列表，存放那些重复的节点的地址

+ 按照节点的地址来遍历哈希表
+ 重复的添加到哈希集合，并将节点添加到record里
+ 难点在于不管是原哈希表还是record里节点的“下一项的地址”的处理。
+ 除了集合外，不需要额外保存节点的空间，思路是改变原列表的每一项的指向，所以是优雅解。

## 暴力解

+ 开一个大于99999的数组，里面存放链表节点。
+ 每一个节点包含自身地址，自身的数字，和下一个节点的地址。
+ 开一个集合，存放已经出现过的数字。
+ 开两个容器，一个为了装不重复的，一个装重复的。
+ 遍历原数组，重复和非重复放相应容器里。
+ 输出两个容器

非常粗暴且不优雅。但是能ac。

# 代码

Python3 使用优雅解，便于大家理解更好的算法。而C++使用暴力解，便于考场快速ac。

## Python3

### 数据结构

+ L 完整链表，用字典表示。
  + 键是字符串也就是address
  + 值是一个列表，包含数值和下一项的地址
+ remove是存放那些重复的节点的地址
+ record是一个集合，记录那些已经出现过的数字的绝对值
+ temp_addr就是为了按地址遍历而设置的变量
+ first_addr 是链表的第一个地址，first_du_addr 是第一个出现重复的节点的地址，默认为-1
+ prev_addr是遍历过程中，记录上一个节点的地址，以便修改上一个节点的下一个节点的指向。

### 算法

+ 按照地址来遍历链表
+ 当temp_addr找到了一个数值
  + 不在record中
    + 将数值添加到record中，同时更新prev_addr = temp_addr
  + 在 record 中
    + 如果是第一个重复的数字
      + 记录first_du_addr = temp_addr
    + 如果不是第一个
      + 将record里记录的上一个地址，使它的下一个等于现在的temp_addr
    + record 添加 temp_addr
    + prev_addr表示的节点的下一项要指向temp_addr的下一项。（即原链表跳过了temp_addr这个节点，因为它是重复的）
+ 记得输出record的时候，要把record最后一位的下一个设置为‘-1’

### 代码本体

```python
# 数据读入
L, remove, record = dict(), list(), set()
first_addr, nodes_num = input().split()
for _ in range(int(nodes_num)):
    begin, num, next_addr = input().split()
    L[begin] = [int(num), next_addr]
    
# 遍历并更新链表
temp_addr = first_addr
first_du_addr = '-1'
while temp_addr != '-1':
    num = L[temp_addr][0]
    if abs(num) not in record:
        record.add(abs(num))
        prev_addr = temp_addr
    else:
        if remove:
            L[remove[-1]][1] = temp_addr
        else:
            first_du_addr = temp_addr
        remove.append(temp_addr)
        L[prev_addr][1] = L[temp_addr][1]
    temp_addr = L[temp_addr][1]

# 输出不重复链表
temp_addr = first_addr
while temp_addr != '-1':
    print(temp_addr, L[temp_addr][0], L[temp_addr][1])
    temp_addr = L[temp_addr][1]

# 输出重复链表
if first_du_addr != '-1':
    L[remove[-1]][1] = '-1'
    print(first_du_addr, L[remove[0]][0], L[remove[0]][1])
    for i in range(1, len(remove)):
        print(L[remove[i - 1]][1], L[remove[i]][0], L[remove[i]][1])
```

## C++

没有任何难度，那么数据结构和算法也不写了，反正一看就看的懂。

```c++
#include <unordered_set>
#include <vector>
#include <iostream>

using namespace std;
struct node {
    int addr;
    int num;
    int next_addr;
};

node Link[1000000];

int main() {
    // 输入
    int first_addr, nodes_num;
    cin >> first_addr >> nodes_num;
    for (int i = 0; i < nodes_num; i++) {
        int addr;
        cin >> addr;
        Link[addr].addr = addr;
        cin >> Link[addr].num >> Link[addr].next_addr;
    }

    // 按需添加
    int temp_addr = first_addr;
    unordered_set<int> record;
    vector<node> no_dup, dup;
    while (temp_addr != -1) {
        int num = abs(Link[temp_addr].num);
        if (record.count(num) == 0) {
            record.insert(num);
            no_dup.push_back(Link[temp_addr]);
        } else dup.push_back(Link[temp_addr]);
        temp_addr = Link[temp_addr].next_addr;
    }

    // 输出
    if (!no_dup.empty()) {
        for (int i = 0; i < (int) no_dup.size() - 1; i++)
            printf("%05d %d %05d\n", no_dup[i].addr, no_dup[i].num, no_dup[i + 1].addr);
        printf("%05d %d -1\n", no_dup[no_dup.size() - 1].addr, no_dup[no_dup.size() - 1].num);
    }
    if (!dup.empty()){
        for (int i = 0; i < (int) dup.size() - 1; ++i)
            printf("%05d %d %05d\n", dup[i].addr, dup[i].num, dup[i + 1].addr);
        printf("%05d %d -1\n", dup[dup.size() - 1].addr, dup[dup.size() - 1].num);
    }
}
```

