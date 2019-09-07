---
title: <Python><PAT> 1153 Decode Registration Card of PAT
date: 2019-09-07 15:47:48
tags:
- PAT
- 题解
- 哈希表
- 排序
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 超级复杂的哈希表与排序
---

# 题目

A registration card number of PAT consists of 4 parts:

- the 1st letter represents the test level, namely, `T` for the top level, `A` for advance and `B` for basic;
- the 2nd - 4th digits are the test site number, ranged from 101 to 999;
- the 5th - 10th digits give the test date, in the form of `yymmdd`;
- finally the 11th - 13th digits are the testee's number, ranged from 000 to 999.

Now given a set of registration card numbers and the scores of the card owners, you are supposed to output the various statistics according to the given queries.

### Input Specification:

Each input file contains one test case. For each case, the first line gives two positive integers $N (≤10^4)$ and *M* (≤100), the numbers of cards and the queries, respectively.

Then *N* lines follow, each gives a card number and the owner's score (integer in [0,100]), separated by a space.

After the info of testees, there are *M* lines, each gives a query in the format `Type Term`, where

- `Type` being 1 means to output all the testees on a given level, in non-increasing order of their scores. The corresponding `Term` will be the letter which specifies the level;
- `Type` being 2 means to output the total number of testees together with their total scores in a given site. The corresponding `Term` will then be the site number;
- `Type` being 3 means to output the total number of testees of every site for a given test date. The corresponding `Term` will then be the date, given in the same format as in the registration card.

### Output Specification:

For each query, first print in a line `Case #: input`, where `#` is the index of the query case, starting from 1; and `input` is a copy of the corresponding input query. Then output as requested:

- for a type 1 query, the output format is the same as in input, that is, `CardNumber Score`. If there is a tie of the scores, output in increasing alphabetical order of their card numbers (uniqueness of the card numbers is guaranteed);
- for a type 2 query, output in the format `Nt Ns` where `Nt` is the total number of testees and `Ns` is their total score;
- for a type 3 query, output in the format `Site Nt` where `Site` is the site number and `Nt` is the total number of testees at `Site`. The output must be in non-increasing order of `Nt`'s, or in increasing order of site numbers if there is a tie of `Nt`.

If the result of a query is empty, simply print `NA`.

### Sample Input:

```in
8 4
B123180908127 99
B102180908003 86
A112180318002 98
T107150310127 62
A107180908108 100
T123180908010 78
B112160918035 88
A107180908021 98
1 A
2 107
3 180908
2 999
```

### Sample Output:

```out
Case 1: 1 A
A107180908108 100
A107180908021 98
A112180318002 98
Case 2: 2 107
3 260
Case 3: 3 180908
107 2
123 2
102 1
Case 4: 2 999
NA
```

# 题解

## 思路

+ 这道题在网上看到暴力解也能过，不过为了更优雅地AC以及应对更大的数据集测试量，这里放的是优雅的解法。
+ 首先题目的意思是给你一堆学生的信息，包含
  + 科目
  + 考场
  + 时间
  + 成绩
  + 编号
+ 然后会向你查询
+ 查询的内容有
  + 给定科目，输出参加这门考试的所有学生的总编号和成绩，成绩降序+编号升序
  + 给定考场，输出这个考场的总人数和总分数
  + 给定考试时间，输出这个时间的考场和当天的考场人数，按照人数降序+考场升序
+ 应对查询问题，最好的方法就是哈希表，所以这题难在数据结构的设计，尤其是对于第三个。

## 数据结构

+ level 是一个三项的列表，这三项对应级别B，A，和T
  + 其中的任意一项，存放考这个级别考试的所有考生
    + 一个考生有编号和成绩两项
    + 对于Python来说，直接[B122,21]表示一个学生，对于C++，使用了一个struct，为了方便排序。C++对于一个level里，存的是一个set，因为可以边插入边排序，降低复杂度
+ 对于第二个查询，site而言，python使用了一个列表，而C++使用了site_num[]和site_score[]两个列表。
  + Python用[人数，分数]来表示一个考成
  + C++ 就是下标和值相对应的两个列表
  + 第二个查询比较简单，不涉及排序
+ date 是最难的。date是一个哈希集，将时间映射到另一个哈希集。另一个哈希集将教室映射到人数。
  + 比如date[970331]是存放97年3月31日的所有考场信息。
  + 一个考场信息也是一个哈希集，代表人数，比如`date[970331][103] = 3`代表在那一天，103教室的所有人数是3。对于Python和C++都是这个设计。
+ 以上方的数据结构，我们所有的查询都可以尽量缩短至`O(1)`。除了最后一个date，需要在查询的时候排序。这也是没办法的事，因为我们的排序依据只有查询完了才知道。

## 算法

+ 读入数据
  + 更新相应的level
  + 更新相应的site
  + 更新相应的date
+ 读入查询
  + 按照情况输出即可
  + 注意NA的情况

## 代码

+ 这道题使用PYTHON会有两个点超时，因此放了两种语言的解。
+ 对于C++来说，有些查询结果是一旦插入就确定好顺序的，比如查询科目的时候。这时候使用了set这种类型，可以保证插入的时候就是有序的。像对于一个时间的所有考场，则必须等到输入完所有的数据才能排序，因为考场人数是最后才能确定的。
+ 因为没有defaultdict，因此C++冗长在判断这一项是否已经存在哈希表当中，使用`count()`函数。

### Python

```python
from collections import defaultdict

num, que = list(map(int, input().split()))
level = [[], [], []]
site = [[0, 0] for _ in range(1000)]
date = defaultdict(dict)
for _ in range(num):
    info, score = input().split()
    if info[0] == "B":
        level[0].append([info, int(score)])
    elif info[0] == "A":
        level[1].append([info, int(score)])
    else:
        level[2].append([info, int(score)])
    site[int(info[1:4])][0] += 1
    site[int(info[1:4])][1] += int(score)
    date[int(info[4:10])][int(info[1:4])] = date[int(info[4:10])].get(int(info[1:4]), 0) + 1

for i in range(que):
    type, term = input().split()
    print("Case %d: %s %s" % (i + 1, type, term))
    if type == "1":
        if term =="B":
            level_index = 0
        elif term == "A":
            level_index = 1
        else:
            level_index = 2
        if not level[level_index]:
            print("NA")
        else:
            level[level_index].sort(key=lambda x: (-x[1], x[0]))
            for i in level[level_index]:
                print(i[0], i[1])
    elif type == "2":
        if site[int(term)][0] == 0:
            print("NA")
        else:
            print(site[int(term)][0], site[int(term)][1])
    else:
        if not date[int(term)]:
            print("NA")
        else:
            ans = sorted([[i, j] for i, j in date[int(term)].items()], key=lambda x: (-x[1], x[0]))
            for i in ans:
                print(i[0], i[1])

```

### C++

```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <set>

using namespace std;
struct stu {
    string info;
    int score;
    bool operator<(const stu &b) const{
        if (score != b.score)
            return score > b.score;
        return info < b.info;
    }
};

struct site{
    int id;
    int num;
    bool operator<(const site &b) const{
        if (num != b.num)
            return num > b.num;
        return id < b.id;
    }
};

int main() {
    int num,que;
    cin>>num>>que;
    set<stu> level[3];
    int site_num[1000] = {0};
    int site_score[1000] = {0};
    unordered_map<int,unordered_map<int,int>> date;
    for (int i = 0 ; i < num ;i ++){
        string info;
        int score;
        cin>>info>>score;
        if (info[0] == 'B')
            level[0].insert({info,score});
        else if (info[0] == 'A')
            level[1].insert({info,score});
        else
            level[2].insert({info,score});
        int temp_site = stoi(info.substr(1,3));
        site_num[temp_site] += 1;
        site_score[temp_site] += score;
        int temp_date = stoi(info.substr(4,6));
        if (date.count(temp_date) == 0)
            date[temp_date][temp_site] = 1;
        else{
            if (date[temp_date].count(temp_site) == 0)
                date[temp_date][temp_site] = 1;
            else
                date[temp_date][temp_site] += 1;
        }
    }
    for (int i = 1; i <= que ; i ++){
        int type;
        string term;
        cin>>type>>term;
        printf("Case %d: %d %s\n" ,i, type, term.c_str());
        if (type == 1){
            int level_index;
            if (term =="B")
                level_index = 0;
            else if ( term == "A")
                level_index = 1;
            else
                level_index = 2;
            if (level[level_index].empty())
                printf("NA\n");
            else{
                for (const auto& it : level[level_index])
                    printf("%s %d\n",it.info.c_str(),it.score);
            }
        }
        else if (type ==2){
            if (site_num[stoi(term)] == 0)
                printf("NA\n");
            else
                printf("%d %d\n",site_num[stoi(term)], site_score[stoi(term)]);
        }
        else{
            if (date.count(stoi(term))==0)
                printf("NA\n");
            else{
                set<site> ans;
                for (auto &it:date[stoi(term)])
                    ans.insert({it.first,it.second});
                for (auto it:ans)
                    printf("%d %d\n",it.id,it.num);
            }
        }
    }
}
```

