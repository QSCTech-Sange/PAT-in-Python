---
title: <Python><PAT> 1055 The World''s Richest
date: 2019-08-27 19:43:08
tags: 
- PAT
- 题解
- 哈希表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 是先排序还是后排序？
---

# 题目

Forbes magazine publishes every year its list of billionaires based on the annual ranking of the world's wealthiest people. Now you are supposed to simulate this job, but concentrate only on the people in a certain range of ages. That is, given the net worths of *N* people, you must find the *M* richest people in a given range of their ages.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive integers: $N\left( \leq 10^{5}\right)$ - the total number of people, and $K\left( \leq 10^{3}\right)$ - the number of queries. Then *N* lines follow, each contains the name (string of no more than 8 characters without space), age (integer in (0, 200]), and the net worth (integer in $\left[-10^{6}, 10^{6}\right]$) of a person. Finally there are *K* lines of queries, each contains three positive integers: $M( \leq 100)$ - the maximum number of outputs, and [`Amin`, `Amax`] which are the range of ages. All the numbers in a line are separated by a space.

### Output Specification:

For each query, first print in a line `Case #X:` where `X` is the query number starting from 1. Then output the *M* richest people with their ages in the range [`Amin`, `Amax`]. Each person's information occupies a line, in the format

```
Name Age Net_Worth
```

The outputs must be in non-increasing order of the net worths. In case there are equal worths, it must be in non-decreasing order of the ages. If both worths and ages are the same, then the output must be in non-decreasing alphabetical order of the names. It is guaranteed that there is no two persons share all the same of the three pieces of information. In case no one is found, output `None`.

### Sample Input:

```in
12 4
Zoe_Bill 35 2333
Bob_Volk 24 5888
Anny_Cin 95 999999
Williams 30 -22
Cindy 76 76000
Alice 18 88888
Joe_Mike 32 3222
Michael 5 300000
Rosemary 40 5888
Dobby 24 5888
Billy 24 5888
Nobody 5 0
4 15 45
4 30 35
4 5 95
1 45 50
```

### Sample Output:

```out
Case #1:
Alice 18 88888
Billy 24 5888
Bob_Volk 24 5888
Dobby 24 5888
Case #2:
Joe_Mike 32 3222
Zoe_Bill 35 2333
Williams 30 -22
Case #3:
Anny_Cin 95 999999
Michael 5 300000
Alice 18 88888
Cindy 76 76000
Case #4:
None
```

# 题解

## 思路

+ 题目让你输出给定年龄范围内的最富有人。
+ 有两种思路
  + 第一种 预排序法
    + 我们先把所有人按照要求的顺序排列，那么年龄是无序的
    + 这时候如果要输出的话，必须把整个列表遍历一遍，然后在年龄范围内的输出一次
  + 第二种 后排序法
    + 还有一种思路就是我们先按年龄存储数据
    + 输出的时候，先按年龄提取出数据，再进行排序
+ 实际操作的时候，对于一个测试点，第一种会快一些，对于其他测试点，第二种会快一点，但偏偏总重要的那个测试点需要靠第一个。这完全看数据给的偏好哪种。
+ 这里排序要求是先财富逆序，然后年龄正序，然后姓名正序。

## 数据结构

+ 在Python里，一个人的信息由一个列表`[name,age,wealth]`表示。
+ 在C++中，一个人的信息由结构体构成。
+ 在预排序法中，有一个叫guys的列表或者说vector，它存放着所有人的信息。
+ 在先排序法中，count记录这次查询输出了的富豪的个数。
+ 在后排序法中，有一个age数组或者说列表
  + 下标是年龄
  + 值是在这个年龄上的所有人，是一个列表或者vector
+ 在后排序法中，有一个ans数组，是存储给定年龄范围内的所有人的信息。

## 算法

### 预排序法

+ 读取数据
+ 把所有人放到guys里
+ 对guys按照题目要求进行排序
+ 读取要输出的信息
+ 先预设count为0并打印元信息
+ 遍历guys的每一个人
  + 当年龄在范围内的时候，打印他
  + 同时增加count
  + 当count 等于num的时候
  + 跳出遍历
+ 如果count还是0
  + 打印None

### 后排序法

+ 建立一长度为201的age数组，值是一个列表
+ 读取数据，将人的信息添加到相应age数组里
+ 读取要输出的数据
+ 查找age数组，把符合年龄范围内的候选人都挑选出来。
+ 对这些人应用堆排序，找到符合的前几个人。
+ 把这些人输出。
+ C++的堆排序有一些复杂，尤其是判断符号还得反一反，真的很麻烦，不建议使用。

## 代码

### Python 预排序法

+ 会有两个点过不了

```python
num_people, num_que = list(map(int, input().split()))

guys = []
for i in range(num_people):
    info = input().split()
    guys.append([info[0], int(info[1]), int(info[2])])
guys.sort(key=lambda x: (-x[2], x[1], x[0]))

for i in range(num_que):
    num, mini, maxi = list(map(int, input().split()))
    count = 0
    print("Case #%d:" % (i + 1))
    for j in guys:
        if mini <= j[1] <= maxi:
            print(j[0], j[1], j[2])
            count += 1
            if count == num:
                break
    if count == 0:
        print("None")
```

### Python 后排序法

+ 会有一个点过不了

```python
from heapq import nsmallest

num_people, num_que = list(map(int, input().split()))
age = [[] for _ in range(201)]
for i in range(num_people):
    info = input().split()
    age[int(info[1])].append([info[0], int(info[1]), int(info[2])])

for i in range(num_que):
    num, mini, maxi = list(map(int, input().split()))
    print("Case #%d:" % (i + 1))
    ans = []
    for j in range(mini, maxi + 1):
        ans.extend(age[j])
    ans = nsmallest(num, ans, key=lambda x: (-x[2], x[1], x[0]))
    for i in ans:
        print(i[0], i[1], i[2])
    if len(ans) == 0:
        print("None")

```

### C++ 前排序法

+ 稳。

```c++
#include <algorithm>
#include <vector>
#include <string>

using namespace std;

struct Guy {
    string name;
    int age;
    int wealth;

    Guy(string a, int b, int c) : name(move(a)), age(b), wealth(c) {};

    bool operator<(const Guy &b) {
        if (wealth != b.wealth)
            return wealth > b.wealth;
        if (age != b.age)
            return b.age > age;
        return b.name > name;
    }
};

int main() {
    int num_people, num_que;
    scanf("%d %d", &num_people, &num_que);
    vector<Guy> guys;
    for (int i = 0; i < num_people; i++) {
        string a;
        a.resize(8);
        int b, c;
        scanf("%s %d %d", &a[0], &b, &c);
        guys.emplace_back(Guy(a, b, c));
    }
    sort(guys.begin(),guys.end());

    for (int i = 0; i < num_que; i++) {
        int num, amin, amax;
        scanf("%d %d %d", &num, &amin, &amax);
        printf("Case #%d:\n", i + 1);
        int count = 0;
        for (auto &it:guys){
            if (it.age >= amin && it.age <= amax){
                printf("%s %d %d\n", it.name.c_str(), it.age, it.wealth);
                count += 1;
            }
            if (count == num)
                break;
        }
        if (count == 0)
            printf("None");
    }
}
```

### C++ 后排序法

+ 会有一个点过不了。C++写堆排序的步骤还是有点点难记的。

```c++
#include <algorithm>
#include <vector>
#include <string>

using namespace std;

struct Guy {
    string name;
    int age;
    int wealth;

    Guy(string a, int b, int c) : name(move(a)), age(b), wealth(c) {};

    bool operator<(const Guy &b) {
        if (wealth != b.wealth)
            return wealth < b.wealth;
        if (age != b.age)
            return b.age < age;
        return b.name < name;
    }
};

int main() {
    int num_people, num_que;
    scanf("%d %d", &num_people, &num_que);
    vector<Guy> age[201];
    for (int i = 0; i < num_people; i++) {
        string a;
        a.resize(8);
        int b, c;
        scanf("%s %d %d", &a[0], &b, &c);
        age[b].emplace_back(Guy(a, b, c));
    }

    for (int i = 0; i < num_que; i++) {
        int num, amin, amax;
        scanf("%d %d %d", &num, &amin, &amax);
        vector<Guy> ans;
        for (int j = amin; j <= amax; j++)
            ans.insert(ans.end(), age[j].begin(), age[j].end());
        make_heap(ans.begin(), ans.end());
        printf("Case #%d:\n", i + 1);
        if (ans.size() == 0)
            printf("None");
        else {
            for (int j = 0; j < num; j++) {
                if (ans.size() != 0) {
                    pop_heap(ans.begin(), ans.end());
                    printf("%s %d %d\n", ans.back().name.c_str(), ans.back().age, ans.back().wealth);
                    ans.pop_back();
                } else
                    break;
            }
        }

    }
}
```

