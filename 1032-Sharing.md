---
title: <Python><PAT> 1032 Sharing
date: 2019-08-24 11:52:39
tags: 
- PAT
- 题解
- 哈希表
- 哈希集合
- 链表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 使用Python会有一个点超时。这道题可以暴力开数组，也可以优雅地使用哈希表和哈希集合。
---

# 题目

To store English words, one method is to use linked lists and store a word letter by letter. To save some space, we may let the words share the same sublist if they share the same suffix. For example, `loading` and `being` are stored as showed in Figure 1.

![fig.jpg](https://images.ptausercontent.com/ef0a1fdf-3d9f-46dc-9a27-21f989270fd4.jpg)

Figure 1

You are supposed to find the starting position of the common suffix (e.g. the position of `i` in Figure 1).

### Input Specification:

Each input file contains one test case. For each case, the first line contains two addresses of nodes and a positive *N* (≤105), where the two addresses are the addresses of the first nodes of the two words, and *N* is the total number of nodes. The address of a node is a 5-digit positive integer, and NULL is represented by −1.

Then *N* lines follow, each describes a node in the format:

```
Address Data Next
```

where`Address` is the position of the node, `Data` is the letter contained by this node which is an English letter chosen from { a-z, A-Z }, and `Next` is the position of the next node.

### Output Specification:

For each case, simply output the 5-digit starting position of the common suffix. If the two words have no common suffix, output `-1` instead.

### Sample Input 1:

```in
11111 22222 9
67890 i 00002
00010 a 12345
00003 g -1
12345 D 67890
00002 n 00003
22222 B 23456
11111 L 00001
23456 e 67890
00001 o 00010
```

### Sample Output 1:

```out
67890
```

### Sample Input 2:

```in
00001 00002 4
00001 a 10001
10001 s -1
00002 a 10002
10002 t -1
```

### Sample Output 2:

```out
-1
```

# 题解

## 思路

+ 我们不关心字母本身是什么，关心的是地址。
+ 建立一个哈希表，键是这个字母的地址，值是下一个字母的地址。
+ 也可以开一个足够大的数组，但是这样不够优雅。
+ 遍历第一个单词，把每一个字母地址添加到一个哈希集中
+ 遍历第二个单词，如果一个字母地址出现在哈希集当中，说明重合了。

## 数据结构

+ next 是一个字典（哈希表）
  + 键是整型的，代表字母地址
  + 值也是整型，代表这个字母下一个字母的地址
+ A 是一个哈希集
  + 存放第一个单词的所有字目的地址
+ addr 是遍历的临时变量

## 算法

+ 都写在思路里了。

## 代码

+ 使用Python的话最后一个点会超时。因此两个语言都写了。要注意使用字符串作为哈希键的话，C++也会超时。应该使用整数作为键，同时输出的时候补0。

### Python

```python
# 读入数据与数据结构初始化
addr_1, addr_2, num_letters = list(map(int, input().split()))
next = dict()
for _ in range(num_letters):
    a, letter, b = input().split()
    next[int(a)] = int(b)

# 将第一个单词出现的地址添加到哈希集和里
addr = addr_1
A = set()
while addr != -1:
    A.add(addr)
    addr = next[addr]

# 遍历第二个单词
addr = addr_2
while addr != -1:
    if addr not in A:
        addr = next[addr]
    else:
        print("%05d" % (int(addr)))
        break
else:
    print(-1)
```

### C++

```c++
#include <iostream>
#include <unordered_map>
#include <unordered_set>
using namespace std;

int main() {
    // 读入数据与数据结构初始化
    int addr_1, addr_2;
    int num_letters;
    cin >> addr_1 >> addr_2 >> num_letters;
    unordered_map<int, int> next;
    unordered_set<int> A;   
    for (int i = 0; i < num_letters; i++) {
        int a, b;
        char temp;
        cin >> a >> temp >> b;
        next[a] = b;
    }
    
    // 将第一个单词出现的地址添加到哈希表
    int addr = addr_1;
    while (addr != -1){
        A.insert(addr);
        addr = next[addr];
    }
    
    // 遍历第二个单词
    addr = addr_2;
    while (addr != -1){
        if (A.count(addr)!=0){
            printf("%05d",addr);
            break;
        }
        addr = next[addr];
    }
    if (addr == -1)
        cout<<"-1"<<endl;
}
```

