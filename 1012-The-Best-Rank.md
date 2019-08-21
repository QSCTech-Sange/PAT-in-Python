---
title: <Python><PAT> 1012 The Best Rank
date: 2019-08-20 14:30:48
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 中档题，看似很简单，但是要把数据结构设计得很优雅，还是需要一些思考和技巧的。
---

# 题目

To evaluate the performance of our first year CS majored students, we consider their grades of three courses only: `C` - C Programming Language, `M` - Mathematics (Calculus or Linear Algrbra), and `E` - English. At the mean time, we encourage students by emphasizing on their best ranks -- that is, among the four ranks with respect to the three courses and the average grade, we print the best rank for each student.

For example, The grades of `C`, `M`, `E` and `A` - Average of 4 students are given as the following:

```
StudentID  C  M  E  A
310101     98 85 88 90
310102     70 95 88 84
310103     82 87 94 88
310104     91 91 91 91
```

Then the best ranks for all the students are No.1 since the 1st one has done the best in C Programming Language, while the 2nd one in Mathematics, the 3rd one in English, and the last one in average.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 2 numbers *N* and *M* (≤2000), which are the total number of students, and the number of students who would check their ranks, respectively. Then *N* lines follow, each contains a student ID which is a string of 6 digits, followed by the three integer grades (in the range of [0, 100]) of that student in the order of `C`, `M` and `E`. Then there are *M* lines, each containing a student ID.

### Output Specification:

For each of the *M* students, print in one line the best rank for him/her, and the symbol of the corresponding rank, separated by a space.

The priorities of the ranking methods are ordered as `A` > `C` > `M` > `E`. Hence if there are two or more ways for a student to obtain the same best rank, output the one with the highest priority.

If a student is not on the grading list, simply output `N/A`.

### Sample Input:

```in
5 6
310101 98 85 88
310102 70 95 88
310103 82 87 94
310104 91 91 91
310105 85 90 90
310101
310102
310103
310104
310105
999999
```

### Sample Output:

```out
1 C
1 M
1 E
1 A
3 A
N/A
```

# 题解

## 思路

+ 这道题很容易从学生入手，以学生为下标建立数组或是哈希表，然后值存储成绩和名次等待
+ 但是这样涉及到排序等问题，复杂度很大，开数组也难以针对6位数的学生数量。
+ 更合理更优雅的办法，是给成绩开数组。
+ 例如，给数学成绩开数组
  + 下标对应成绩
  + 值是一个集合，对应考到这个数学成绩的学生的id们
+ 这样，每个数组就101个大小，average如果只计算它们三门成绩相加的话就是301大小，如果算三门成绩平均值的话就是还是101个大小。题目的测试集没有对这个做区分，比如99,99,98的平均分取整会和99,98,98一样都是98，算和的话就是前者排名高，算平均数且直接取整的话就是后者高。但是题目不区分，所以无关紧要，用哪个都能AC。
+ 读取数据后，开一个rank哈希表，记录一个学号的最好排名
+ 遍历四个成绩数组
  + 按照四个成绩优先级遍历
    + 遍历的时候，对每个成绩的所有人
    + 按需更新它们的rank
+ 输出

## 数据结构

+ A，C，M，E 是四门成绩，都是数组
  + 下标代表成绩
  + 值是一个列表，代表这门课考到这个成绩的所有人
+ rank，best是两个哈希表
  + 键是学生id
  + 值是最优排名和科目

## 算法

+ 读入学号和成绩的时候
  + 对应成绩的数组添加学号
+ 更新rank和best的时候
  + 令temp_rank为1，临时排名
  + 从下标最大的（也就是成绩最好的）开始遍历一门科目的成绩
    + 对考到这个成绩的每一个人来说
      + 如果temp_rank比现在的rank[id]要小，更新rank和subject
    + 更新temp_sum，加上考到这个成绩的人数

## 代码

因为使用Python能AC，因此只留了Python的代码。

```python
# 成绩数组
A, C, M, E = [set() for _ in range(301)], [set() for _ in range(101)], [set() for _ in range(101)], [set() for _ in range(101)]

# 读取数据
num_stu, num_inq = list(map(int, input().split()))
for _ in range(num_stu):
    info = input().split()
    C[int(info[1])].add(info[0])
    M[int(info[2])].add(info[0])
    E[int(info[3])].add(info[0])
    A[int(info[1]) + int(info[2]) + int(info[3])].add(info[0])

# 排名和最好科目
rank, best_subject = dict(), dict()

# 更新排名的函数
def update_rank(subject, sub_str):
    temp_rank = 1
    for i in range(len(subject)-1, -1, -1):
        for id in subject[i]:
            if id not in rank or temp_rank < rank[id]:
                rank[id] = temp_rank
                best_subject[id] = sub_str
        temp_rank += len(subject[i])

# 按优先级更新排名
update_rank(A, 'A')
update_rank(C, 'C')
update_rank(M, 'M')
update_rank(E, 'E')

# 输出
for _ in range(num_inq):
    id = input()
    if id in rank:
        print(rank[id], best_subject[id])
    else:
        print("N/A")

```

