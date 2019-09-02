---
title: 1083 List Grades
date: 2019-09-02 11:55:15
tags: 
- PAT
- 题解
- 排序
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 憨憨排序题
---

# 题目

Given a list of *N* student records with name, ID and grade. You are supposed to sort the records with respect to the grade in non-increasing order, and output those student records of which the grades are in a given interval.

### Input Specification:

Each input file contains one test case. Each case is given in the following format:

```
N
name[1] ID[1] grade[1]
name[2] ID[2] grade[2]
... ...
name[N] ID[N] grade[N]
grade1 grade2
```

where `name[i]` and `ID[i]` are strings of no more than 10 characters with no space, `grade[i]` is an integer in [0, 100], `grade1` and `grade2` are the boundaries of the grade's interval. It is guaranteed that all the grades are **distinct**.

### Output Specification:

For each test case you should output the student records of which the grades are in the given interval [`grade1`, `grade2`] and are in non-increasing order. Each student record occupies a line with the student's name and ID, separated by one space. If there is no student's grade in that interval, output `NONE` instead.

### Sample Input 1:

```in
4
Tom CS000001 59
Joe Math990112 89
Mike CS991301 100
Mary EE990830 95
60 100
```

### Sample Output 1:

```out
Mike CS991301
Mary EE990830
Joe Math990112
```

### Sample Input 2:

```in
2
Jean AA980920 60
Ann CS01 80
90 95
```

### Sample Output 2:

```out
NONE
```

# 题解

## 思路

+ 读数据
+ 排序
+ 输出
+ 完事儿嗷

## 数据结构

+ students 存放所有的学生，是一个列表
+ 一个student用一个三项列表表示
  + 第一项是名字
  + 第二项是科目
  + 第三项是成绩
+ low,high 是成绩上下限

## 算法

+ 读入students数据
+ 读入成绩的上下限
+ 将students按照上下限清理一下
+ 如果students为空，输出NONE
+ 否则，将students排序并输出

## 代码

+ 由于使用Python能AC，因此只放了Python解。

```python
num, students = int(input()), []
for _ in range(num):
    students.append(input().split())
low, high = list(map(int, input().split()))
students = [i for i in students if low <= int(i[2]) <= high]
if len(students) == 0:
    print("NONE")
else:
    students.sort(key=lambda x: int(x[2]), reverse=True)
    for student in students:
        print(student[0], student[1])

```

