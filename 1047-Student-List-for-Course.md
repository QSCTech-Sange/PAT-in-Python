---
title: <Python><PAT> 1047 Student List for Course
date: 2019-08-27 10:03:31
tags: 
- PAT
- 题解
- 哈希表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 使用数组替代哈希表。
---

# 题目

Zhejiang University has 40,000 students and provides 2,500 courses. Now given the registered course list of each student, you are supposed to output the student name lists of all the courses.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 numbers: *N* (≤40,000), the total number of students, and *K* (≤2,500), the total number of courses. Then *N* lines follow, each contains a student's name (3 capital English letters plus a one-digit number), a positive number *C* (≤20) which is the number of courses that this student has registered, and then followed by *C* course numbers. For the sake of simplicity, the courses are numbered from 1 to *K*.

### Output Specification:

For each test case, print the student name lists of all the courses in increasing order of the course numbers. For each course, first print in one line the course number and the number of registered students, separated by a space. Then output the students' names in alphabetical order. Each name occupies a line.

### Sample Input:

```in
10 5
ZOE1 2 4 5
ANN0 3 5 2 1
BOB5 5 3 4 2 1 5
JOE4 1 2
JAY9 4 1 2 5 4
FRA8 3 4 2 5
DON2 2 4 5
AMY7 1 5
KAT3 3 5 4 2
LOR6 4 2 4 1 5
```

### Sample Output:

```out
1 4
ANN0
BOB5
JAY9
LOR6
2 7
ANN0
BOB5
FRA8
JAY9
JOE4
KAT3
LOR6
3 1
BOB5
4 7
BOB5
DON2
FRA8
JAY9
KAT3
LOR6
ZOE1
5 9
AMY7
ANN0
BOB5
DON2
FRA8
JAY9
KAT3
LOR6
ZOE1
```

# 题解

## 思路

+ 将由学生作主键的信息转为由课程作主键的信息。
+ 通常会想到一个哈希表，用课程作下标。
+ 但是这道题用数组就可以了，因为课程本身就是有序数字。
+ 一个课程的值是一个列表，存储上这门课的学生
+ 按格式存储
+ 注意！使用Python的话，要注意最后的输出得判断一下是不是无人选这门课。
+ 排序并输出即可。

## 数据结构

+ courses 是一个二维数组，含义是存放所有课程的参与学生
  + 下标0直接忽略
  + 其他下标就代表课程号
    + 值是一个列表，包含所有的上这门课的学生。

## 算法

+ 照着格式输入
+ 照着格式输出

## 代码

+ 由于使用Python能够AC，因此只放了Python的代码。

```python
num_stu, num_courses = list(map(int, input().split()))
courses = [[] for i in range(num_stu + 1)]
for _ in range(num_stu):
    info = input().split()
    for i in info[2:]:
        courses[int(i)].append(info[0])
for i in range(1, num_courses + 1):
    if courses[i]:
        print(i, len(courses[i]))
        print("\n".join(sorted(courses[i])))
    else:
        print(i, "0")
```

