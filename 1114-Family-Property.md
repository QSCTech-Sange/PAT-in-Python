---
title: <Python><PAT> 1114 Family Property
date: 2019-09-04 15:11:14
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 并查集的较难应用
---

# 题目

This time, you are supposed to help us collect the data for family-owned property. Given each person's family members, and the estate（房产）info under his/her own name, we need to know the size of each family, and the average area and number of sets of their real estate.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤1000). Then *N* lines follow, each gives the infomation of a person who owns estate in the format:

`ID` `Father` `Mother` $k\;Child_1 \cdots Child_k\;M_{estate}$ `Area`


where `ID` is a unique 4-digit identification number for each person; `Father` and `Mother` are the `ID`'s of this person's parents (if a parent has passed away, `-1` will be given instead); *k* (0≤*k*≤5) is the number of children of this person; $Child_i$‘s are the `ID`'s of his/her children; $M_{estate}$ is the total number of sets of the real estate under his/her name; and `Area` is the total area of his/her estate.

### Output Specification:

For each case, first print in a line the number of families (all the people that are related directly or indirectly are considered in the same family). Then output the family info in the format:

`ID` `M` $AVG_{sets} AVG_{area}$

where `ID` is the smallest ID in the family; `M` is the total number of family members; $AVG_{sets}$ is the average number of sets of their real estate; and $AVG_{area}$ is the average area. The average numbers must be accurate up to 3 decimal places. The families must be given in descending order of their average areas, and in ascending order of the ID's if there is a tie.

### Sample Input:

```in
10
6666 5551 5552 1 7777 1 100
1234 5678 9012 1 0002 2 300
8888 -1 -1 0 1 1000
2468 0001 0004 1 2222 1 500
7777 6666 -1 0 2 300
3721 -1 -1 1 2333 2 150
9012 -1 -1 3 1236 1235 1234 1 100
1235 5678 9012 0 1 50
2222 1236 2468 2 6661 6662 1 300
2333 -1 3721 3 6661 6662 6663 1 100
```

### Sample Output:

```out
3
8888 1 1.000 1000.000
0001 15 0.600 100.000
5551 4 0.750 100.000
```

# 题解

## 思路

+ 统计有哪些家族并且统计家族房产
+ 那么很容易想到并查集
+ 但是这题难点在不仅要合并，还要同时统计房产数量，家族人数和房产面积，并且输出的时候还要按照最小id的原则输出
+ 所以我们要在并查集的基础上做一些处理。
+ 我们要记录下每个人拥有的房地产面积和数量，以及开一个members数组。members  是记录每个组合的人数，如果一个人是一个组合的老大，那么这个数不为0,否则都是0。一开始members所有值都是0
+ 读信息的时候，把这个人，父母，以及孩子都添加进一个并查集里面。利用findFather和Union。这里运用了路径压缩。
+ 把这个人，父母，以及孩子添加进people里，people 记录所有出现过的人。
+ 将people从小到大排序，遍历people
  + 如果这个人的根在members 里面是0
    + 说明这个组合还没添加过，而这个人一定是这个组合里面最小的id，把它添加到ans里面。
  + 如果这个人的根不是自己，即自己不是老大
    + 那么把自己的房产数量和房产面积算到老大头上
  + 这个组合的member数量加一
+ 遍历ans，把里面的每一个人的信息，换成一个答案组合。
  + 即，自己的id
  + 自己的根（老大）的成员数
  + 自己的根（老大）的房地产数量/成员数
  + 自己的根（老大）的房地产面积/成员数
+ 把ans按照题目要求的顺序排序
+ 输出ans，即可

## 数据结构

+ father 是一个并查集，下标是id，值是它的父亲，默认自己是自己的父亲
+ sets 是记录每个人的房产数量
+ are 是记录每个人的房产面积
+ members  是记录每个组合的人数，如果一个人是一个组合的老大，那么这个数不为0,否则都是0
+ ans 存放答案，一开始存放每个组合的最小id，后来存放所有答案组合的信息。
  + 一个答案组合存放
    + 一个组的最小id
    + 组合总人数
    + 平均房产数量
    + 平均房产面积
+ info 是读取一行输入的信息
+ family_members  是一行信息中的所有成员，包括自己，父母和他的孩子。
+ people 记录数据中所有的人

## 算法

+ 都写在思路里了。

## 代码

+ 由于使用Python能够AC，因此只放了Python的解。

```python
n = int(input())
father = [i for i in range(10001)]
sets = [0 for i in range(10001)]
area = [0 for i in range(10001)]
members = [0 for _ in range(10001)]
people = set()
ans = []


def findFather(id):
    if father[id] != id:
        father[id] = findFather(father[id])
    return father[id]


def Union(a, b):
    father[findFather(a)] = findFather(b)


for _ in range(n):
    info = list(map(int, input().split()))
    family_members = [int(info[0]), int(info[1]), int(info[2])]
    child_num = int(info[3])
    family_members += info[4:4 + child_num]
    area[family_members[0]] = info[-1]
    sets[family_members[0]] = info[-2]
    for j in family_members:
        if j != -1:
            Union(j, family_members[0])
            people.add(j)

for i in sorted(list(people)):
    if members[findFather(i)] == 0:
        ans.append(i)
    if i != findFather(i):
        sets[findFather(i)] += sets[i]
        area[findFather(i)] += area[i]
    members[findFather(i)] += 1

for i in range(len(ans)):
    ans[i] = [ans[i], members[findFather(ans[i])], sets[findFather(ans[i])] / members[findFather(ans[i])],
              area[findFather(ans[i])] / members[findFather(ans[i])]]

ans.sort(key=lambda x: (-x[3], x[0]))
print(len(ans))
for i in ans:
    print("%04d %d %.3f %.3f" % (i[0], i[1], i[2], i[3]))

```

