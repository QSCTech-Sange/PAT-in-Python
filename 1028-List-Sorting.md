---
title: <Python><PAT> 1028 List Sorting
date: 2019-08-23 19:29:22
tags: 
- PAT
- 题解
- 排序
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 用Python十秒就能做出来但是会有一个点超时。
---

# 题目

Excel can sort records according to any column. Now you are supposed to imitate this function.

### Input Specification:

Each input file contains one test case. For each case, the first line contains two integers *N* (≤105) and *C*, where *N* is the number of records and *C* is the column that you are supposed to sort the records with. Then *N* lines follow, each contains a record of a student. A student's record consists of his or her distinct ID (a 6-digit number), name (a string with no more than 8 characters without space), and grade (an integer between 0 and 100, inclusive).

### Output Specification:

For each test case, output the sorting result in *N* lines. That is, if *C* = 1 then the records must be sorted in increasing order according to ID's; if *C* = 2 then the records must be sorted in non-decreasing order according to names; and if *C* = 3 then the records must be sorted in non-decreasing order according to grades. If there are several students who have the same name or grade, they must be sorted according to their ID's in increasing order.

### Sample Input 1:

```in
3 1
000007 James 85
000010 Amy 90
000001 Zoe 60
```

### Sample Output 1:

```out
000001 Zoe 60
000007 James 85
000010 Amy 90
```

### Sample Input 2:

```in
4 2
000007 James 85
000010 Amy 90
000001 Zoe 60
000002 James 98
```

### Sample Output 2:

```out
000010 Amy 90
000002 James 98
000007 James 85
000001 Zoe 60
```

### Sample Input 3:

```in
4 3
000007 James 85
000010 Amy 90
000001 Zoe 60
000002 James 90
```

### Sample Output 3:

```out
000001 Zoe 60
000007 James 85
000002 James 90
000010 Amy 90
```

# 题解

## 思路

+ 就排序呗，还能咋地

## 数据结构

+ records 记录所有数据，是一个列表（动态数组）
+ 表示一个record时，c++使用了结构体而Python使用了列表

## 算法

+ 排序就调用内置函数就行了。快且方便。

## 代码

使用Python会有一个点超时，建议使用C++。但是Python代码是真短真爽啊。

### Python

```python
num_records, sort_base = list(map(int, input().split()))
records = []
for i in range(num_records):
    id, name, score = input().split()
    records.append([id, name, int(score)])
records.sort(key=lambda x: (x[sort_base - 1],x[0]))
for i in records:
    print(" ".join(list(map(str,i))))
```

### C++

原理一模一样但是行数要多了很多，唉。

```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

struct Record {
    string id;
    string name;
    int score;

    Record(string a, string b, int c) : id(a), name(b), score(c) {};
};

bool id(const Record &a, const Record &b) {
    return a.id < b.id;
}

bool name(const Record &a, const Record &b) {
    if (a.name != b.name)
        return a.name < b.name;
    else
        return a.id < b.id;
}

bool score(const Record &a, const Record &b) {
    if (a.score != b.score)
        return a.score < b.score;
    else
        return a.id < b.id;
}

int main() {
    int num_records, sort_base;
    cin >> num_records >> sort_base;
    vector<Record> records;
    for (int i = 0; i < num_records; i++) {
        string id, name;
        int score;
        cin >> id >> name >> score;
        records.emplace_back(Record(id, name, score));
    }
    switch (sort_base) {
        case 1:
            sort(records.begin(), records.end(), id);
            break;
        case 2:
            sort(records.begin(), records.end(), name);
            break;
        default:
            sort(records.begin(),records.end(),score);
    }
    for (auto &i:records)
        cout<<i.id<<" "<<i.name<<" "<<i.score<<endl;
}

```

