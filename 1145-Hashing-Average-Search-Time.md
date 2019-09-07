---
title: <Python><PAT> 1145 Hashing - Average Search Time
date: 2019-09-06 16:26:01
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 大坑的哈希表
---

# 题目

The task of this problem is simple: insert a sequence of distinct positive integers into a hash table first. Then try to find another sequence of integer keys from the table and output the average search time (the number of comparisons made to find whether or not the key is in the table). The hash function is defined to be $H(key) = key \% TSize$where $TSize$ is the maximum size of the hash table. Quadratic probing (with positive increments only) is used to solve the collisions.

Note that the table size is better to be prime. If the maximum size given by the user is not prime, you must re-define the table size to be the smallest prime number which is larger than the size given by the user.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 3 positive numbers: MSize, N, and M, which are the user-defined table size, the number of input numbers, and the number of keys to be found, respectively. All the three numbers are no more than 104. Then N distinct positive integers are given in the next line, followed by M positive integer keys in the next line. All the numbers in a line are separated by a space and are no more than 105.

### Output Specification:

For each test case, in case it is impossible to insert some number, print in a line `X cannot be inserted.` where `X` is the input number. Finally print in a line the average search time for all the M keys, accurate up to 1 decimal place.

### Sample Input:

```in
4 5 4
10 6 4 15 11
11 4 15 2
```

### Sample Output:

```out
15 cannot be inserted.
2.8
```

# 题解

## 思路

+ 题目非常坑
+ 它的表面意思是，让你模拟一个哈希表
+ 先给你一个size，让你找到这个size的下一个质数作为哈希表的size
+ 这个没问题
+ 然后给你N个数，让你把它们插到哈希表里面，给定了哈希函数和二次探针法。
+ 这个也没问题，遇到插不进去的无视就好
+ 坑的来了，给你M个数，让你查询这M个数在不在这个哈希表里并计算平均比较量。如果查询不到，输出这个数不能被插入
+ 坑点来了，到底是插入还是查询？
+ 而实际上，插入也不对，查询也不对，就是两者的结合
+ 题目想要你做的任务是
  + 对每一个数，查询它在不在列表里
    + 如果它在列表里，那么只记录比较次数就可以
    + 如果沿着二次探针法，所有的坑都填满了别的数，那么输出这个数不能被插入，并记录比较次数
    + 如果遇到了一个坑可以放这个数但是这个数不在这个坑里面，记录现在的比较次数，但是**不要**把它插进去。
    + 抽象吧～

## 数据结构

+  size 是哈希表的尺寸
+ hash_table 是一个哈希表
+ compare 记录比较次数

## 算法

+ 先找到size后面下一个质数
+ 将原数推进哈希表，使用二次探针法。j即探针，一开始是0
+ 然后对查询序列使用思路的方式
+ 输出即可

## 代码

```python
size, num_input, num_que = list(map(int, input().split()))

if size == 1 or size == 2:
    size = 2
elif size != 3:
    if size % 2 == 0:
        size += 1
    while True:
        for i in range(3, int(size ** 0.5 + 1)):
            if size % i == 0:
                size += 2
                break
        else:
            break

hash_table = [None for _ in range(size)]
for i in list(map(int, input().split())):
    for j in range(size):
        key = (i + j ** 2) % size
        if not hash_table[key]:
            hash_table[key] = i
            break

compare = 0
for i in list(map(int, input().split())):
    for j in range(size):
        key = (i + j ** 2) % size
        if hash_table[key] == i:
            compare += (j + 1)
            break
        if not hash_table[key]:
            compare += (j + 1)
            break
    else:
        print(i, "cannot be inserted.")
        compare += (size + 1)

print("%.1f" % (compare / num_que))

```

