---
title: <Python><PAT> 1075 PAT Judge
date: 2019-08-31 17:35:01
tags: 
- PAT
- 题解
- 排序
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 行云流水使用Python排序，输出。然而，超时了。
---

# 题目

The ranklist of PAT is generated from the status list, which shows the scores of the submissions. This time you are supposed to generate the ranklist for PAT.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 3 positive integers, $N(\leq10^4)$, the total number of users, *K* (≤5), the total number of problems, and $M(\leq10^5)$, the total number of submissions. It is then assumed that the user id's are 5-digit numbers from 00001 to *N*, and the problem id's are from 1 to *K*. The next line contains *K* positive integers `p[i]` (`i`=1, ..., *K*), where `p[i]` corresponds to the full mark of the i-th problem. Then *M* lines follow, each gives the information of a submission in the following format:

```
user_id problem_id partial_score_obtained
```

where `partial_score_obtained` is either −1 if the submission cannot even pass the compiler, or is an integer in the range [0, `p[problem_id]`]. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, you are supposed to output the ranklist in the following format:

```
rank user_id total_score s[1] ... s[K]
```

where `rank` is calculated according to the `total_score`, and all the users with the same `total_score` obtain the same `rank`; and `s[i]` is the partial score obtained for the `i`-th problem. If a user has never submitted a solution for a problem, then "-" must be printed at the corresponding position. If a user has submitted several solutions to solve one problem, then the highest score will be counted.

The ranklist must be printed in non-decreasing order of the ranks. For those who have the same rank, users must be sorted in nonincreasing order according to the number of perfectly solved problems. And if there is still a tie, then they must be printed in increasing order of their id's. For those who has never submitted any solution that can pass the compiler, or has never submitted any solution, they must NOT be shown on the ranklist. It is guaranteed that at least one user can be shown on the ranklist.

### Sample Input:

```in
7 4 20
20 25 25 30
00002 2 12
00007 4 17
00005 1 19
00007 2 25
00005 1 20
00002 2 2
00005 1 15
00001 1 18
00004 3 25
00002 2 25
00005 3 22
00006 4 -1
00001 2 18
00002 1 20
00004 1 15
00002 4 18
00001 3 4
00001 4 2
00005 2 -1
00004 2 0
```

### Sample Output:

```out
1 00002 63 20 25 - 18
2 00005 42 20 0 22 -
2 00007 42 - 25 - 17
2 00001 42 18 18 4 2
5 00004 40 15 0 25 -
```

# 题解

## 思路

- 这道题很复杂
- 但是使用Python就会很优雅
- 可是会有一个点超时
- 其实这道题每个点都不难，只是拼在一起显得有些庞大
- 首先就是一个读入成绩，计算总成绩并排名输出的题
- 主要是有一些细节，我们来理一理。
- 首先是最复杂的，对于**成绩**
- 我们把成绩分为三个状态
  - 完全没做（这里设为-2）
  - 做了，但是编译错误(-1)
  - 做了，且有分
- 在计算总成绩的时候，编译错误（-1）的要按0计。
- 在输出的时候，判断要不要输出的标准是
  - 所有题目都是没做或编译错误（all in (-1 -2)）
- 输出的时候
  - 对于编译错误(-1)，输出0
  - 对于没做的题(-2)，输出“-”
- 我们可以默认给每个学生都是-2,然后每次读成绩取已有成绩和读到成绩的max即可
- 接着就是对于**排序**
  - 第一原则是总分从大到小
  - 第二原则是答对满分的题目总量从大到小
  - 第三原则是id从小到大
- 最后关于排名的计算
- 我们初始化一个rank等于0
- 当我们排好序并开始遍历的时候，有一个循环变量，假设是`i`
- 如果`i==0`或者遍历着的这个人的总成绩和上一个人不一样
- 那么`rank = i + 1`
- 否则`rank`不变。

## 数据结构

+ full 是一个列表，表示每道题目的满分
  + 下标是(题目序号-1)
  + 值是满分
  + 即，full[0] = 10 表示第一题的满分是10
+ rank 记录排名
+ total_score 记录每个人的总成绩
+ scores 记录每个人的成绩，题号下标为真实题号-1
  - 即，`scores[3][5] = 10` 含义是id为3的人的第6题得分是10
  - 对于Python而言，scores是一个字典。
  - 对于C++而言，scores 是一个数组。
  - 两者可以互换，只是为了cover两种做法所以这么写。
+ 对C++而言
+ User表示一个人的结构体
  + 包含了id
  + 总成绩
  + 满分题目个数
  + 重载了小于号以便比较
+ users 是一个vector，存放了所有的考生，以User的形式存放，它用来被排序
+ should_print 记录一个学生该不该被打印。
+ 虽然C++数据结构复杂了很多，但是计算的思想其实是一样的。

## 算法

### Python

+ 读入full，初始化scores，令其对每个考生的每个科目成绩都是-2
+ 计算总成绩，计入total_score
+ 排序。其中答对满分的个数由`-len([i for i in range(num_pro) if full[i] == scores[x][i]])`来表示。即新建一个列表，遍历这个人的分数，如果一门成绩等于满分，那么添加进这个列表。最后统计列表的项数。所以总共的排序用一行就可以完成。
+ 其实total_score 也可以在排序里用列表生成式完成，但是因为最后还要输出和，不如先存下来。
+ 输出，rank计算方法在思路里，最后每门的成绩用成绩列表连成字符串，同时替换-2和-1。

### C++

+ 读入full，初始化scores，should_print，令其对每个考生的每个科目成绩都是-2，每个考生都是不该被打印。
+ 读入成绩的时候，如果读到了非-1的成绩，那么更新这个考生的should_print为True
+ 读入成绩按照max(原成绩，新读到的)这样更新即可。
+ 统计每个考生的满分题数和总分，建立一个user，并添加到users这个vector里面。
+ 对vector进行排序。
+ 输出。

## 代码

+ 这道题使用Python会有一个点超时。

### Python

```python
from collections import defaultdict
# 数据结构初始化
num_users, num_pro, num_sub = list(map(int, input().split()))
full = list(map(int, input().split()))
scores = defaultdict(lambda: [-2 for _ in range(num_pro)])
for _ in range(num_sub):
    id, pro, score = input().split()
    scores[id][int(pro) - 1] = max(scores[id][int(pro) - 1], int(score))
# 计算总成绩
total_score = defaultdict(int)
for id in scores.keys():
    total_score[id] = sum([i for i in scores[id] if i != -2 and i != -1])
# 排序
ids = sorted(scores.keys(),
             key=lambda x: (-total_score[x], -len([i for i in range(num_pro) if full[i] == scores[x][i]]), int(x)))
# 输出
rank = 0
for i, id in enumerate(ids):
    if set(scores[id]).issubset({-1, -2}):
        break
    if rank == 0 or total_score[id] != total_score[ids[i - 1]]:
        rank = i + 1
    print(rank, id, total_score[id], end=" ")
    ans = " ".join(list(map(str, scores[id]))).replace("-2", "-").replace("-1", "0")
    print(ans)

```

### C++

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct User {
    int id;
    int total_score;
    int num_full;

    bool operator<(const User &b) {
        if (total_score != b.total_score)
            return b.total_score < total_score;
        if (num_full != b.num_full)
            return b.num_full < num_full;
        return id < b.id;
    }
};


int main() {
    int num_users, num_pro, num_sub;
    scanf("%d%d%d", &num_users, &num_pro, &num_sub);
    // 题目的满分
    int full[num_pro];
    for (int i = 0; i < num_pro; i++)
        scanf("%d", &full[i]);
	// 该不该打印
    bool should_print[num_users + 1];
    for (int i = 0;i<=num_users;i++)
        should_print[i]= false;
	// 每个人每道题的分
    int scores[num_users + 1][num_pro];
    for (int i = 0; i <= num_users; i++) {
        for (int j = 0; j < num_pro; j++)
            scores[i][j] = -2;
    }
    // 读取数据，并更新最高分，更新该不该打印
    for (int i = 0; i < num_sub; i++) {
        int id, pro, score;
        scanf("%d%d%d", &id, &pro, &score);
        if (score != -1)
            should_print[id] = true;
        scores[id][pro - 1] = max(scores[id][pro - 1], score);
    }
    // 统计总分和满分题目数量，并添加到vector中
    vector<User> users;
    for (int i = 1; i <= num_users; i++) {
        int temp_full, temp_total;
        temp_full = 0;
        temp_total = 0;
        for (int j = 0; j < num_pro; j++) {
            if (scores[i][j] > 0) {
                temp_total += scores[i][j];
                if (full[j] == scores[i][j])
                    temp_full += 1;
            }
        }
        users.emplace_back(User{i, temp_total, temp_full});
    }
    // 排序
    sort(users.begin(), users.end());
	// 输出
    int rank = 0;
    for (int i = 0; i < num_users; i++) {
        if (!should_print[users[i].id])
            break;
        if (rank == 0 or users[i].total_score != users[i - 1].total_score)
            rank = i + 1;
        printf("%d %05d %d", rank, users[i].id, users[i].total_score);
        for (int j = 0; j < num_pro; j++) {
            if (scores[users[i].id][j] == -2)
                printf(" -");
            else if (scores[users[i].id][j] == -1)
                printf(" 0");
            else
                printf(" %d", scores[users[i].id][j]);
        }
        printf("\n");
    }
}

```

