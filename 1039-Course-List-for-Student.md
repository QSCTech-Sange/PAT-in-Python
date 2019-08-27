---
title: <Python><PAT> 1039 Course List for Student
date: 2019-08-25 15:58:43
tags: 
- PAT
- 题解
- 哈希表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 建议先用python写再转为C++解答的题。对python歧视严重。
---

# 题目

Zhejiang University has 40000 students and provides 2500 courses. Now given the student name lists of all the courses, you are supposed to output the registered course list for each student who comes for a query.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive integers: *N* (≤40,000), the number of students who look for their course lists, and *K* (≤2,500), the total number of courses. Then the student name lists are given for the courses (numbered from 1 to *K*) in the following format: for each course *i*, first the course index *i* and the number of registered students *N**i* (≤200) are given in a line. Then in the next line, *N**i* student names are given. A student name consists of 3 capital English letters plus a one-digit number. Finally the last line contains the *N* names of students who come for a query. All the names and numbers in a line are separated by a space.

### Output Specification:

For each test case, print your results in *N* lines. Each line corresponds to one student, in the following format: first print the student's name, then the total number of registered courses of that student, and finally the indices of the courses in increasing order. The query results must be printed in the same order as input. All the data in a line must be separated by a space, with no extra space at the end of the line.

### Sample Input:

```in
11 5
4 7
BOB5 DON2 FRA8 JAY9 KAT3 LOR6 ZOE1
1 4
ANN0 BOB5 JAY9 LOR6
2 7
ANN0 BOB5 FRA8 JAY9 JOE4 KAT3 LOR6
3 1
BOB5
5 9
AMY7 ANN0 BOB5 DON2 FRA8 JAY9 KAT3 LOR6 ZOE1
ZOE1 ANN0 BOB5 JOE4 JAY9 FRA8 DON2 AMY7 KAT3 LOR6 NON9
```

### Sample Output:

```out
ZOE1 2 4 5
ANN0 3 1 2 5
BOB5 5 1 2 3 4 5
JOE4 1 2
JAY9 4 1 2 4 5
FRA8 3 2 4 5
DON2 2 4 5
AMY7 1 5
KAT3 3 2 4 5
LOR6 4 1 2 4 5
NON9 0
```

# 题解

## 思路

+ 题目给每门课的学生信息，让你输出每个学生的课的信息
+ 有种sql的感觉
+ 所以我们可以考虑用哈希表来做一个学生到课程集的映射
+ 即，哈希表的键是学生名，值是一个列表，表示学生上的所有课。
+ 按要求输出即可

## 数据结构

+ students 是一个字典（哈希表）
  + 键是学生的姓名
  + 值是一个列表，代表这个学生选的所有课，是一个列表(vector)

## 算法

+ 读入数据
+ 将数据填到相应的哈希表学生当中
+ 读入输出数据
+ 排序相应学生的列表
+ 输出

## 代码

### Python3

+ Python代码会有三个点过不了。就算考虑到最后的输出可能多了空格，也会有两个点过不了。就算考虑到一个课可能没学生上，那两个点还是过不了。所以这道题用Python反而会迷失在细节当中。应该是因为确实是有测试数据没有按照格式来分行导致的。所以我这里就简单写一下代码说明一下思路。还挺清楚挺简洁的，不是吗？

```python
from collections import defaultdict

students = defaultdict(list)
_, num_class = input().split()
for _ in range(int(num_class)):
    id, num_stu = input().split()
    for i in input().split():
        students[i].append(id)
for i in input().split():
    print(i, len(students[i]), " ".join(sorted(students[i])))

```

### C++

+ C++ 可以全部AC。

```c++
#include<iostream>
#include<unordered_map>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int num_stu, num_cls;
    scanf("%d %d", &num_stu, &num_cls);
    unordered_map<string, vector<int>> students;
    for (int i = 0; i < num_cls; i++) {
        int cls_id, num_cls_stu;
        scanf("%d %d", &cls_id, &num_cls_stu);
        for (int j = 0; j < num_cls_stu; j++) {
            string name;
            cin >> name;
            students[name].push_back(cls_id);
        }
    }
    for (int i = 0; i < num_stu; i++) {
        string name;
        cin >> name;
        cout << name << " " << students[name].size();
        if (students[name].size() != 0) {
            sort(students[name].begin(), students[name].end());
            for (auto &it:students[name])
                printf(" %d", it);
        }
        printf("\n");
    }
}
```

