---
title: <Python><PAT> 1137 Final Grading
date: 2019-09-05 21:32:45
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 排序问题
---

# 题目

or a student taking the online course "Data Structures" on China University MOOC (http://www.icourse163.org/), to be qualified for a certificate, he/she must first obtain no less than 200 points from the online programming assignments, and then receive a final grade no less than 60 out of 100. The final grade is calculated by $G = (G_{mid-term} \times 40 \% + G_{final} \times 60 \%)$ if $G_{mid-term} > G_{final}$, or $G_{final}$ will be taken as the final grade *G*. Here $G_{mid-term}$ and $G_{final}$ are the student's scores of the mid-term and the final exams, respectively.

The problem is that different exams have different grading sheets. Your job is to write a program to merge all the grading sheets into one.

### Input Specification:

Each input file contains one test case. For each case, the first line gives three positive integers: P , the number of students having done the online programming assignments; M, the number of students on the mid-term list; and N, the number of students on the final exam list. All the numbers are no more than 10,000.

Then three blocks follow. The first block contains P online programming scores $G_P$'s; the second one contains M mid-term scores $G_{mid-term}$’s; and the last one contains N final exam scores $G_{final}$'s. Each score occupies a line with the format: `StudentID Score`, where `StudentID` is a string of no more than 20 English letters and digits, and `Score` is a nonnegative integer (the maximum score of the online programming is 900, and that of the mid-term and final exams is 100).

### Output Specification:

For each case, print the list of students who are qualified for certificates. Each student occupies a line with the format:

`StudentID`  $G_P G_{mid-term} G_{final} G$

If some score does not exist, output "−1" instead. The output must be sorted in descending order of their final grades (*G* must be rounded up to an integer). If there is a tie, output in ascending order of their `StudentID`'s. It is guaranteed that the `StudentID`'s are all distinct, and there is at least one qullified student.

### Sample Input:

```in
6 6 7
01234 880
a1903 199
ydjh2 200
wehu8 300
dx86w 220
missing 400
ydhfu77 99
wehu8 55
ydjh2 98
dx86w 88
a1903 86
01234 39
ydhfu77 88
a1903 66
01234 58
wehu8 84
ydjh2 82
missing 99
dx86w 81
```

### Sample Output:

```out
missing 400 -1 99 99
ydjh2 200 98 82 88
dx86w 220 88 81 84
wehu8 300 55 84 84
```

# 题解

## 思路

+ 题目的意思是
+ 给你每个学生的平时分，期中分，期末分
+ 让你计算他的总成绩并将所有合格的学生排序并输出
+ 总成绩的计算条件是
  + 如果期中分大于期末分
    + 期中分4成，期末分6成
  + 否则
    + 就是期末分
+ 所有合格的学生指平时分超过200且总成绩大于等于60
+ 排序的原则是总成绩从高到低，然后是姓名升序
+ 注意！！！很坑的一点，总成绩排序的时候，排序依据而不只是输出依据已经是四舍五入过得了。举例来说，A的总成绩是83.8，B的总成绩是84，但是A的排名还是比B高，因为排名的依据的成绩已经是四舍五入过的了，所以A和B都是一样的成绩，按照姓名排列A比B前面。
+ 而输出的时候，如果一个人的一项成绩是空的，那么输出-1。
+ 我们只需要默认每一样成绩都是-1就好了。在计算总成绩的时候，遇到-1的当0来看即可。
+ 由于题目给的数据是先平时，后期中，后期末的格式，所以为了方便索引到用户，我们可以建立几个哈希表来对应各项成绩。

## 数据结构

+ online 对应平时成绩，是一个哈希表
  + 键是姓名
  + 值是成绩，默认是-1
+ mid 对应期中成绩，是一个哈希表
  + 键是姓名
  + 值是成绩，默认是-1
+ final 对应期末成绩，是一个哈希表，
  + 键是姓名，
  + 值是成绩，默认是-1
+ G 对应总成绩，是一个哈希表，
  + 键是姓名，
  + 值是成绩，默认是-1
+ name_lists 是所有人的名单

## 算法

+ 初始化数据结构
+ 读取每张成绩单上的数据，添加到相应的哈希表里
+ 计算name_lists
+ 遍历name_lists的每一项，计算他的总成绩。如果一项成绩是-1的话要当作0来计算
+ 筛选要输出的人的名单，按照在线成绩和总成绩
+ 排序这些人，按照总成绩和姓名
+ 输出，即可

## 代码

+ 由于使用Python可以AC，因此只放了Python的题解。

```python
from collections import defaultdict

num_online, num_mid, num_final = list(map(int, input().split()))
online = defaultdict(lambda: -1)
mid = defaultdict(lambda: -1)
final = defaultdict(lambda: -1)
G = defaultdict(lambda: -1)

for _ in range(num_online):
    name, score = input().split()
    online[name] = int(score)
for _ in range(num_mid):
    name, score = input().split()
    mid[name] = int(score)
for _ in range(num_final):
    name, score = input().split()
    final[name] = int(score)

name_lists = set(list(online.keys()) + list(mid.keys()) + list(final.keys()))
for name in name_lists:
    mid_scroe = mid[name] if mid[name] != -1 else 0
    final_score = final[name] if final[name] != -1 else 0
    G[name] = round(mid_scroe * 0.4 + final_score * 0.6) if mid_scroe > final_score else final_score

lists = [i for i in name_lists if online[i] >= 200 and G[i] >= 60]
lists.sort(key=lambda x: (-G[x], x))

for i in lists:
    print("%s %d %d %d %d" % (i, online[i], mid[i], final[i], G[i]))

```

