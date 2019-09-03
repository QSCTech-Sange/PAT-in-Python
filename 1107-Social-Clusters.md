---
title: <Python><PAT> 1107 Social Clusters
date: 2019-09-03 20:11:43
tags:
- PAT
- 题解
- 并查集
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 算算你的社交圈？
---

# 题目

When register on a social network, you are always asked to specify your hobbies in order to find some potential friends with the same hobbies. A **social cluster** is a set of people who have some of their hobbies in common. You are supposed to find all the clusters.

### Input Specification:

Each input file contains one test case. For each test case, the first line contains a positive integer *N* (≤1000), the total number of people in a social network. Hence the people are numbered from 1 to *N*. Then *N* lines follow, each gives the hobby list of a person in the format:
$$
K_i:h_i[1] h_i[2] ... h_i[K_i]
$$
where $K_i(>0)$ is the number of hobbies, and $h_i[j]$ is the index of the *j*-th hobby, which is an integer in [1, 1000].

### Output Specification:

For each case, print in one line the total number of clusters in the network. Then in the second line, print the numbers of people in the clusters in non-increasing order. The numbers must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

```in
8
3: 2 7 10
1: 4
2: 5 3
1: 4
1: 3
1: 4
4: 6 8 1 5
1: 4
```

### Sample Output:

```out
3
4 3 1
```

# 题解

## 思路

+ 题目里给出所有人的所有兴趣爱好
+ 如果有两个人只要有一个兴趣爱好相同，它们就算作一个组
+ 问你所有人总共分成几组，并且每组的人数
+ 这道题很明显用并查集即可

## 数据结构

+ hobby 是一个列表。
  + 下标是这个兴趣的id
  + 值是这个兴趣的第一次喜欢的人
+ father 记录每个人的上一层
  + 当自己是老大时，father[i] = i，
  + 它是一个并查集
+ isroot 记录每个人是否是老大以及这个团体的数量
  + 当值为0时，说明这个人不是老大
  + 当值大于0时，说明这个人是老大，且值是团伙数量

## 算法

+ 并查集的核心——找上级
  + 这里采用了路径压缩，更高效
  + 每个人的直系上级就是Father[i]
  + 但是他的上级不一定就是这个团伙的老大
  + 所以要反复进行father[i]直到找到老大。
  + 通过递归完成操作
+ 我们先读取每一个人的每一个兴趣
  + 当这个兴趣对应在hobby中没有人喜欢过
    + 那么hobby[这个兴趣] = 这个人
  + 否则，需要把这个人加入到喜欢这个兴趣的团体当中。
    + 方法是，这到这个人的老大，和这个团体的老大
    + 让这个人的老大的老大等于这个团体的老大
+ 我们遍历所有人
  + 每个人的老大的isroot += 1
+ 遍历isroot
  + 不是0的通通添加进答案里
+ 排序isroot并输出

## 代码

+ 由于使用Python能够AC，因此只放了Python的代码。

```python
hobby = [0 for i in range(1001)]
n = int(input())
father = [i for i in range(n + 1)]
isRoot = [0 for i in range(n + 1)]


def findFather(son):
    if son != father[son]:
        father[son] = findFather(father[son])
    return father[son]

for i in range(n):
    hobbies = list(map(int, input().split()[1:]))
    for j in hobbies:
        if hobby[j] == 0:
            hobby[j] = i + 1
        else:
            father[findFather(i + 1)] = findFather(hobby[j])

for i in range(n):
    isRoot[findFather(i + 1)] += 1
ans = []
for i in isRoot:
    if i != 0:
        ans.append(i)
print(len(ans))
print(" ".join(list(map(str, sorted(ans,reverse=True)))))

```

