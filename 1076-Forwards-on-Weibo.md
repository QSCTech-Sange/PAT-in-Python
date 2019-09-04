---
title: <Python><PAT> 1076 Forwards on Weibo
date: 2019-09-01 11:48:24
tags: 
- PAT
- 题解
- BFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 爱豆与粉丝
---

# 题目

Weibo is known as the Chinese version of Twitter. One user on Weibo may have many followers, and may follow many other users as well. Hence a social network is formed with followers relations. When a user makes a post on Weibo, all his/her followers can view and forward his/her post, which can then be forwarded again by their followers. Now given a social network, you are supposed to calculate the maximum potential amount of forwards for any specific user, assuming that only *L* levels of indirect followers are counted.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive integers: *N* (≤1000), the number of users; and *L* (≤6), the number of levels of indirect followers that are counted. Hence it is assumed that all the users are numbered from 1 to *N*. Then *N* lines follow, each in the format:

```
M[i] user_list[i]
```

where `M[i]` (≤100) is the total number of people that `user[i]` follows; and `user_list[i]` is a list of the `M[i]` users that followed by `user[i]`. It is guaranteed that no one can follow oneself. All the numbers are separated by a space.

Then finally a positive *K* is given, followed by *K* `UserID`'s for query.

### Output Specification:

For each `UserID`, you are supposed to print in one line the maximum potential amount of forwards this user can trigger, assuming that everyone who can view the initial post will forward it once, and that only *L* levels of indirect followers are counted.

### Sample Input:

```in
7 3
3 2 3 4
0
2 5 6
2 3 1
2 3 4
1 4
1 5
2 2 6
```

### Sample Output:

```out
4
5
```

# 题解

## 思路

+ 题目的意思是，给你一个社交网络。
+ 里面有很多人，每个人既可以关注别人，也可以被别人关注
+ 换言之，既可以成为很多人的爱豆，也可以成为很多人的粉丝
+ 作为一个粉丝，当一个爱豆发消息时会传播这个消息给自己的所有粉丝
+ 题目的问题是，给你一个人，当这个人作为爱豆发了一个消息时，最多有多少个人能看到。即粉丝的粉丝的粉丝……的总数。
+ 但是多了一个层数的限制。
+ 既然看到了层数，不难想到广度优先遍历。
+ 只要遍历到了相应的层数即可。

## 数据结构

+ fans 是一个列表（数组），记录所有人的所有粉丝
  + 下标是这个人的id
  + 值是一个列表（vector），存放这个人的所有粉丝
+ que 是一个队列，存放待遍历的id们
+ visited 是一个列表，记录每个人有没有被访问过
+ count 记录要输出的粉丝总数
+ level 记录已经到了哪一层

## 算法

+ 题目初始给的数据是第`i`行数据，表示id为`i`的人关注了哪些人
+ 我们先转换成，对关注的那些人，粉丝都添加`i`。
+ 对每个查询的id
+ 先设visited 对 除了id以外的所有人都为False，对id 为True
+ 设level = 1,count = 0
+ 设队列只有(id)
+ 然后开始一个while循环，循环进行条件是当队列不为空且level小于等于给定的max_level
  + 循环内部
  + 首先我们记录队列里有多少个id。这些id表示都是第level层的粉丝。
  + 对这些id，如果没有访问过
    + 将它设为访问过
    + 同时把它添加到队列里
    + 并且count += 1
  + 最后level再加1

## 代码

+ 这道题Python会有一个点超时。因此也放了C++的代码。

### Python3

```python
from queue import Queue
# 数据读入
num_users, max_level = list(map(int, input().split()))
fans = [[] for _ in range(num_users + 1)]
for i in range(num_users):
    for id in list(map(int, input().split()[1:])):
        fans[id].append(i + 1)
# 对每个查询进行BFS
for id in list(map(int, input().split()[1:])):
    visited = [False for _ in range(num_users + 1)]
    visited[id] = True
    count = 0
    que = Queue()
    que.put(id)
    level = 1
    while not que.empty() and level <= max_level:
        for i in range(que.qsize()):
            star = que.get()
            for fan in fans[star]:
                if not visited[fan]:
                    visited[fan] = True
                    que.put(fan)
                    count += 1
        level += 1
    print(count)
```

### C++

```c++
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

int main() {
    // 数据读入
    int num_users, max_level;
    scanf("%d%d", &num_users, &max_level);
    vector<int> fans[num_users + 1];
    for (int i = 1; i <= num_users; i++) {
        int num_friends;
        scanf("%d", &num_friends);
        for (int j = 0; j < num_friends; j++) {
            int friend_temp;
            scanf("%d", &friend_temp);
            fans[friend_temp].push_back(i);
        }
    }
    // 对每个输入进行BFS
    int num_que;
    scanf("%d", &num_que);
    for (int i = 0; i < num_que; i++) {
        int id;
        scanf("%d", &id);
        bool visited[num_users + 1];
        for (int j = 0; j <= num_users; j++)
            visited[j] = false;
        visited[id] = true;
        int count = 0;
        int level = 1;
        queue<int> que;
        que.push(id);
        while (!que.empty() and level <= max_level) {
            int length = que.size();
            for (int j = 0; j < length; j++) {
                int star = que.front();
                que.pop();
                for (auto &fan:fans[star]) {
                    if (!visited[fan]) {
                        visited[fan] = true;
                        que.push(fan);
                        count += 1;
                    }
                }
            }
            level += 1;
        }
        printf("%d\n", count);
    }
}

```

