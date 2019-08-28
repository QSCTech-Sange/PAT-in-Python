---
title: 1062 Talent and Virtue
date: 2019-08-28 21:40:22
tags:
---

# 题目

About 900 years ago, a Chinese philosopher Sima Guang wrote a history book in which he talked about people's talent and virtue. According to his theory, a man being outstanding in both talent and virtue must be a "sage（圣人）"; being less excellent but with one's virtue outweighs talent can be called a "nobleman（君子）"; being good in neither is a "fool man（愚人）"; yet a fool man is better than a "small man（小人）" who prefers talent than virtue.

Now given the grades of talent and virtue of a group of people, you are supposed to rank them according to Sima Guang's theory.

### Input Specification:

Each input file contains one test case. Each case first gives 3 positive integers in a line: *N* (≤105), the total number of people to be ranked; *L* (≥60), the lower bound of the qualified grades -- that is, only the ones whose grades of talent and virtue are both not below this line will be ranked; and *H* (<100), the higher line of qualification -- that is, those with both grades not below this line are considered as the "sages", and will be ranked in non-increasing order according to their total grades. Those with talent grades below *H* but virtue grades not are cosidered as the "noblemen", and are also ranked in non-increasing order according to their total grades, but they are listed after the "sages". Those with both grades below *H*, but with virtue not lower than talent are considered as the "fool men". They are ranked in the same way but after the "noblemen". The rest of people whose grades both pass the *L* line are ranked after the "fool men".

Then *N* lines follow, each gives the information of a person in the format:

```
ID_Number Virtue_Grade Talent_Grade
```

where `ID_Number` is an 8-digit number, and both grades are integers in [0, 100]. All the numbers are separated by a space.

### Output Specification:

The first line of output must give *M* (≤*N*), the total number of people that are actually ranked. Then *M* lines follow, each gives the information of a person in the same format as the input, according to the ranking rules. If there is a tie of the total grade, they must be ranked with respect to their virtue grades in non-increasing order. If there is still a tie, then output in increasing order of their ID's.

### Sample Input:

```in
14 60 80
10000001 64 90
10000002 90 60
10000011 85 80
10000003 85 80
10000004 80 85
10000005 82 77
10000006 83 76
10000007 90 78
10000008 75 79
10000009 59 90
10000010 88 45
10000012 80 100
10000013 90 99
10000014 66 60
```

### Sample Output:

```out
12
10000013 90 99
10000012 80 100
10000003 85 80
10000011 85 80
10000004 80 85
10000007 90 78
10000006 83 76
10000005 82 77
10000002 90 60
10000014 66 60
10000008 75 79
10000001 64 90
```

# 题解

## 思路

+ 是一个排序问题
+ 总体来说，道德和才能分数都在及格线以上的才被统计。
+ 按照圣人，君子，愚人，小人这样排。
+ 在每种人里面，按照(分数总分，道德分，姓名)这个优先级排列。其中，前两个是倒序，最后一个是正序。
+ 几种人的划分标准是：
  + 圣人：talent和virtue 都在high 之上
  + 君子：virtue在high之上但是talent在high之下
  + 愚人：virtue和talent都在high之下，但是virtue大等talent
  + 小人：最后一种情况

## 数据结构

+ num_people 总人数，low,high分别是及格线和高分线。
+ sages,noble,fool,small是四个数组，存放圣人，君子，笨蛋和小人。
+ num_ranked 记录两项都及格的人数。
+ 对C++来说，一个人是一个结构体，并重载了比较函数。
+ 对Python来说，一个人是一个列表，依次是id,道德分和能力分。

## 算法

+ 其实就排序算法，排序算法也调用内置的。唯一要写的就是排序的比较。照着题目的意思来就行了。

## 代码

+ 这道题使用Python会有两个点过不了，建议使用C++。方法和思路都是一样的。

### Python3

```python
num_people, low, high = list(map(int, input().split()))
sages, noble, fool, small, num_ranked = [], [], [], [], 0
for i in range(num_people):
    id, virtue, talent = list(map(int, input().split()))
    if virtue >= low and talent >= low:
        num_ranked += 1
        if virtue >= high and talent >= high:
            sages.append([id, virtue, talent])
        elif virtue >= high:
            noble.append([id, virtue, talent])
        elif virtue >= talent:
            fool.append([id, virtue, talent])
        else:
            small.append([id, virtue, talent])

sages.sort(key=lambda x: (-x[1] - x[2], -x[1], x[0]))
noble.sort(key=lambda x: (-x[1] - x[2], -x[1], x[0]))
fool.sort(key=lambda x: (-x[1] - x[2], -x[1], x[0]))
small.sort(key=lambda x: (-x[1] - x[2], -x[1], x[0]))

print(num_ranked)
for i in sages:
    print(i[0], i[1], i[2])
for i in noble:
    print(i[0], i[1], i[2])
for i in fool:
    print(i[0], i[1], i[2])
for i in small:
    print(i[0], i[1], i[2])

```

###　C++

+ 因为sort始终是从小到达排的，所以我们对重载的小于号里面实际上存放的是大于号的比较。这样按照sort的从小到大就是我们希望的从大到小。

```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

struct Person {
    int id;
    int virtue;
    int talent;

    bool operator<(const Person &b) {
        if (virtue + talent != b.virtue + b.talent)
            return virtue + talent > b.virtue + b.talent;
        if (virtue != b.virtue)
            return virtue > b.virtue;
        return id < b.id;
    }
};

int main() {
    int num_people, low, high, num_ranked;
    num_ranked = 0;
    scanf("%d %d %d", &num_people, &low, &high);
    vector<Person> sages;
    vector<Person> nobles;
    vector<Person> fool;
    vector<Person> small;
    for (int i = 0; i < num_people; i++) {
        int id, virtue, talent;
        scanf("%d%d%d", &id, &virtue, &talent);
        if (virtue >= low and talent >= low) {
            num_ranked += 1;
            struct Person temp{id, virtue, talent};
            if (virtue >= high and talent >= high)
                sages.push_back(temp);
            else if (virtue >= high)
                nobles.push_back(temp);
            else if (virtue >= talent)
                fool.push_back(temp);
            else
                small.push_back(temp);
        }
    }
    sort(sages.begin(), sages.end());
    sort(nobles.begin(), nobles.end());
    sort(fool.begin(), fool.end());
    sort(small.begin(), small.end());

    printf("%d\n",num_ranked);
    for (auto &it:sages)
        printf("%08d %d %d\n",it.id,it.virtue,it.talent);
    for (auto &it:nobles)
        printf("%08d %d %d\n",it.id,it.virtue,it.talent);
    for (auto &it:fool)
        printf("%08d %d %d\n",it.id,it.virtue,it.talent);
    for (auto &it:small)
        printf("%08d %d %d\n",it.id,it.virtue,it.talent);
}
```

