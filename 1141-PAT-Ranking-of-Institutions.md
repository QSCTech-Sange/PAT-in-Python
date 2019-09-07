---
title: <Python><PAT> 1141 PAT Ranking of Institutions
date: 2019-09-06 11:36:38
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 排序问题
---

# 题目

After each PAT, the PAT Center will announce the ranking of institutions based on their students' performances. Now you are asked to generate the ranklist.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer $N (≤10^5)$, which is the number of testees. Then N lines follow, each gives the information of a testee in the following format:

```
ID Score School
```

where `ID` is a string of 6 characters with the first one representing the test level: `B` stands for the basic level, `A` the advanced level and `T` the top level; `Score` is an integer in [0, 100]; and `School` is the institution code which is a string of no more than 6 English letters (case insensitive). Note: it is guaranteed that `ID` is unique for each testee.

### Output Specification:

For each case, first print in a line the total number of institutions. Then output the ranklist of institutions in nondecreasing order of their ranks in the following format:

```
Rank School TWS Ns
```

where `Rank` is the rank (start from 1) of the institution; `School` is the institution code (all in lower case); ; `TWS` is the **total weighted score** which is defined to be the integer part of `ScoreB/1.5 + ScoreA + ScoreT*1.5`, where `ScoreX` is the total score of the testees belong to this institution on level `X`; and `Ns` is the total number of testees who belong to this institution.

The institutions are ranked according to their `TWS`. If there is a tie, the institutions are supposed to have the same rank, and they shall be printed in ascending order of `Ns`. If there is still a tie, they shall be printed in alphabetical order of their codes.

### Sample Input:

```in
10
A57908 85 Au
B57908 54 LanX
A37487 60 au
T28374 67 CMU
T32486 24 hypu
A66734 92 cmu
B76378 71 AU
A47780 45 lanx
A72809 100 pku
A03274 45 hypu
```

### Sample Output:

```out
5
1 cmu 192 2
1 au 192 3
3 pku 100 1
4 hypu 81 2
4 lanx 81 2
```

# 题解

## 思路

+ 就是排序一把梭
+ 用哈希表来存储学校
+ 然后就没有然后了

## 数据结构

+ Python里，schools 是一个哈希表，将键映射到一个列表
  + 列表第一项是B的总成绩
  + 第二项是A
  + 第三项是T
  + 第四项是TWS 总加权成绩
  + 第五项是这个学校的总人数
+ C++里将schools哈希表，映射到了一个结构体上。

## 算法

+ 读入数据
+ 计算加权总成绩
+ 排序
+ 输出

## 代码

+ 这道题使用Python最后两个点有时候会超时，概率还比较大。因此也放了C++的题解。

### Python

```python
from collections import defaultdict

num = int(input())
schools = defaultdict(lambda :[0,0,0,0,0])

for _ in range(num):
    name, score, ins = input().split()
    if name[0] == "B":
        schools[ins.lower()][0] += int(score)
    elif name[0] == "A":
        schools[ins.lower()][1] += int(score)
    else:
        schools[ins.lower()][2] += int(score)
    schools[ins.lower()][4] += 1


for ins in schools.keys():
    schools[ins][3] = int(schools[ins][0] / 1.5 + schools[ins][1] + schools[ins][2] * 1.5)

all_school = sorted(schools, key=lambda x: (-schools[x][3], schools[x][4], x))

print(len(all_school))
rank = 0
for i, ins in enumerate(all_school):
    if i == 0 or schools[ins][3] != schools[all_school[i - 1]][3]:
        rank = i + 1
    print(rank, ins, schools[ins][3], schools[ins][4])

```

### C++

```c++
#include <iostream>
#include <algorithm>
#include <unordered_map>
#include <vector>

using namespace std;

struct School {
    string name;
    int A = 0;
    int B = 0;
    int T = 0;
    int TBS = 0;
    int Ns = 0;

    bool operator<(const School &b) {
        if (TBS != b.TBS)
            return TBS > b.TBS;
        if (Ns != b.Ns)
            return Ns < b.Ns;
        return name < b.name;
    };
};


int main() {
    int num;
    cin >> num;
    unordered_map<string, School> schools;
    for (int i = 0; i < num; i++) {
        string name, ins;
        int score;
        cin >> name >> score >> ins;
        transform(ins.begin(), ins.end(), ins.begin(), ::tolower);
        if (name[0] == 'A') {
            if (schools.count(ins) == 0)
                schools[ins] = School{ins, score, 0, 0, 0, 0};
            else
                schools[ins].A += score;
        } else if (name[0] == 'B') {
            if (schools.count(ins) == 0)
                schools[ins] = School{ins, 0, score, 0, 0};
            else
                schools[ins].B += score;
        } else {
            if (schools.count(ins) == 0)
                schools[ins] = School{ins, 0, 0, score, 0};
            else
                schools[ins].T += score;
        }
        schools[ins].Ns += 1;
    }

    for (auto &it:schools)
        it.second.TBS = int(it.second.A + it.second.B / 1.5 + it.second.T * 1.5);

    vector<School>  ans;
    for (auto &it:schools)
        ans.push_back(it.second);

    sort(ans.begin(),ans.end());
    cout << ans.size() << endl;
    int rank = 0;
    for (int i = 0 ; i < ans.size();i++) {
        if (i == 0 or ans[i].TBS != ans[i-1].TBS)
            rank = i + 1;
        printf("%d %s %d %d\n", rank, ans[i].name.c_str(), ans[i].TBS, ans[i].Ns);
    }
}
```

