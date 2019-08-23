---
title: <Python><PAT> 1025 PAT Ranking
date: 2019-08-23 11:37:49
tags: 
- PAT
- 题解
- 排序
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 排序问题，难度适中
---

# 题目

Programming Ability Test (PAT) is organized by the College of Computer Science and Technology of Zhejiang University. Each test is supposed to run simultaneously in several places, and the ranklists will be merged immediately after the test. Now it is your job to write a program to correctly merge all the ranklists and generate the final rank.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive number *N* (≤100), the number of test locations. Then *N* ranklists follow, each starts with a line containing a positive integer *K* (≤300), the number of testees, and then *K* lines containing the registration number (a 13-digit number) and the total score of each testee. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, first print in one line the total number of testees. Then print the final ranklist in the following format:

```
registration_number final_rank location_number local_rank
```

The locations are numbered from 1 to *N*. The output must be sorted in nondecreasing order of the final ranks. The testees with the same score must have the same rank, and the output must be sorted in nondecreasing order of their registration numbers.

### Sample Input:

```in
2
5
1234567890001 95
1234567890005 100
1234567890003 95
1234567890002 77
1234567890004 85
4
1234567890013 65
1234567890011 25
1234567890014 100
1234567890012 85
```

### Sample Output:

```out
9
1234567890005 1 1 1
1234567890014 1 2 1
1234567890001 3 1 2
1234567890003 3 1 2
1234567890004 5 1 4
1234567890012 5 2 2
1234567890002 7 1 5
1234567890013 8 2 3
1234567890011 9 2 4
```

# 题解

## 思路

+ 给一个学生建一个类
+ 包含id，分数，教室号，本地排名和总排名
+ 建一个列表（数组），存放所有的学生类型
+ 先读取每间教室的数据，添加到总学生列表里
+ 每间教室排序，添加到本地排名
+ 再给所有人排序，添加到总排名

## 数据结构

+ Student 是一个类（结构体），包含
  + id
  + 分数
  + 教室编号
  + 本地排名
  + 总排名
  + 对于C++语言，需要添加构造函数和运算符重载，python直接使用lambda即可
+ all_students是一个列表（数组），存放所有的Student。
+ num_class 是教室数量
+ num_all_stu 是总学生
+ num_cls_stu 是这件教室学生

## 算法

+ 如何更新排名？
  + 在本地的环节中，首先对相应的范围进行排序。因为每次添加的新学生都是all_students的最后几项。
  + c++的sort可以指定范围，而Python就只能使用列表拼接，这样会增加复杂度。
  + c++排序的比较写在了类的运算符重载当中，Python直接使用lambda。为了使逆序正序统一，可以使用成绩的**负数**作为第一项，学号作为第二项，这样都是进行**升序**排列。
  + 排序好后，就可以更新rank了。只要遍历项和遍历项的前一项成绩不同，那么就rank+1。相同就等于前一人的rank。
  + 对于Python，可以直接使用index大法。这样相同成绩的都选择第一个下标。
  + c++ 将ranking写成了一个函数，Python直接使用排序+index一把梭。

## 代码

这道题使用Python3 会超时一个点，建议使用C++来做题嗷。

### Python

```python
class Student:
    def __init__(self, id, score, classroom):
        self.id = id
        self.score = score
        self.classroom = classroom
        self.local_rank = None
        self.all_rank = None


all_students = []
num_class = int(input())
num_all_stu = 0

# 读取数据并更新教室排名
for classroom in range(num_class):
    num_cls_stu = int(input())
    for _ in range(num_cls_stu):
        id, score = input().split()
        all_students.append(Student(id, int(score), classroom + 1))
    all_students = all_students[:num_all_stu] + sorted(all_students[num_all_stu:num_all_stu + num_cls_stu],
                                                       key=lambda x: x.score, reverse=True)
    for i in range(num_all_stu, num_all_stu + num_cls_stu):
        all_students[i].local_rank = [i.score for i in all_students[num_all_stu:num_all_stu + num_cls_stu]].index(
            all_students[i].score) + 1
    num_all_stu += num_cls_stu

# 更新总排名
all_students.sort(key=lambda x: (-x.score, x.id))
for i in range(len(all_students)):
    all_students[i].all_rank = [i.score for i in all_students].index(all_students[i].score) + 1
    
# 输出结果
print(num_all_stu)
for i in all_students:
    print(i.id, i.all_rank, i.classroom, i.local_rank)

```

### C++

```c++
#include <iostream>
#include <string>
#include <utility>
#include <vector>
#include <algorithm>

using namespace std;

// 学生类，包含了构造函数和便于比较的运算符重载
struct Student {
    string id;
    int score, classroom;
    int local_rank = 0;
    int all_rank = 0;

    Student(string id, int score, int classroom) : id(move(id)), score(score), classroom(classroom) {};

    bool operator<(const Student &b) {
        if (score > b.score)
            return true;
        else return score == b.score && id < b.id;
    }
};

// 排好序以后，进行排名的读入
void ranking(vector<Student> &all_students, int start, int end, const string& rank_type) {
    int rank = 1;
    for (int i = start; i < end; i++) {
        if (i == 0 or all_students[i - 1].score != all_students[i].score) {
            all_students[i].all_rank = rank_type == "all" ? rank : all_students[i].all_rank;
            all_students[i].local_rank = rank_type == "local" ? rank : all_students[i].local_rank;
        } else {
            all_students[i].all_rank = rank_type == "all" ? all_students[i - 1].all_rank:all_students[i].all_rank;
            all_students[i].local_rank = rank_type == "local" ? all_students[i - 1].local_rank:all_students[i].local_rank;
        }
        rank += 1;
    }
}

int main() {
    vector<Student> all_students;
    int num_all_stu = 0;
    int num_class;
    cin >> num_class;
    
    // 读入信息以及本场排名
    for (int classroom = 1; classroom <= num_class; classroom++) {
        int num_cls_stu;
        cin >> num_cls_stu;
        for (int i = 0; i < num_cls_stu; i++) {
            string id;
            int score;
            cin >> id >> score;
            all_students.emplace_back(Student(id, score, classroom));
        }
        sort(all_students.begin() + num_all_stu, all_students.end());
        ranking(all_students, num_all_stu, num_all_stu + num_cls_stu, "local");
        num_all_stu += num_cls_stu;
    }
    
    // 总体排名
    sort(all_students.begin(), all_students.end());
    ranking(all_students, 0, all_students.size(), "all");
    
    // 输出
    cout << num_all_stu << endl;
    for (auto &i:all_students)
        cout << i.id << " " << i.all_rank << " " << i.classroom << " " << i.local_rank << endl;
}
```

