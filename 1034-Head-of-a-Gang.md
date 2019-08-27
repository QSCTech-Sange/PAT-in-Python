---
title: <Python><PAT> 1034 Head of a Gang
date: 2019-08-24 18:25:03
tags: 
- PAT
- 题解
- DFS
- BFS
- 并查集
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 本质上还是求连通分量，加入了一些细节。
---

# 题目

One way that the police finds the head of a gang is to check people's phone calls. If there is a phone call between *A* and *B*, we say that *A* and *B* is related. The weight of a relation is defined to be the total time length of all the phone calls made between the two persons. A "Gang" is a cluster of more than 2 persons who are related to each other with total relation weight being greater than a given threthold *K*. In each gang, the one with maximum total weight is the head. Now given a list of phone calls, you are supposed to find the gangs and the heads.

### Input Specification:

Each input file contains one test case. For each case, the first line contains two positive numbers *N* and *K* (both less than or equal to 1000), the number of phone calls and the weight threthold, respectively. Then *N* lines follow, each in the following format:

```
Name1 Name2 Time
```

where `Name1` and `Name2` are the names of people at the two ends of the call, and `Time` is the length of the call. A name is a string of three capital letters chosen from `A`-`Z`. A time length is a positive integer which is no more than 1000 minutes.

### Output Specification:

For each test case, first print in a line the total number of gangs. Then for each gang, print in a line the name of the head and the total number of the members. It is guaranteed that the head is unique for each gang. The output must be sorted according to the alphabetical order of the names of the heads.

### Sample Input 1:

```in
8 59
AAA BBB 10
BBB AAA 20
AAA CCC 40
DDD EEE 5
EEE DDD 70
FFF GGG 30
GGG HHH 20
HHH FFF 10
```

### Sample Output 1:

```out
2
AAA 3
GGG 3
```

### Sample Input 2:

```in
8 70
AAA BBB 10
BBB AAA 20
AAA CCC 40
DDD EEE 5
EEE DDD 70
FFF GGG 30
GGG HHH 20
HHH FFF 10
```

### Sample Output 2:

```out
0
```

 # 题解

## 思路

+ 本质上还是求连通分量的题

+ 所以DFS，BFS和并查集都可以做

+ 偷懒就只放了DFS的解，也是最常用的解

+ 题目比普通的求连通分量复杂一点，这里解释一下为什么。

+ 首先下标都是用户名字，所以考虑用哈希表而不是数组来存储通讯记录

+ 其次给的数据可能是这种

+ ```
  AAA BBB 10
  BBB AAA 20
  ```

+ 我们想要的结果是，AAA 到 BBB 的通讯量是30,反之也是30

+ 这样就导向了一种新的数据结构，就是`defaultdict(lambda: defaultdict(int))`。这样就相当于，对每个人分配一个包含每个人的数组，值是分钟数。而且因为是defaultdict(int)，所以直接使用+=号就行，非常方便

+ 最后一点是不能使用传统的dfs再进行回溯，这样会导致计算团伙总人数出现重复计算。我建议的解决方法是判断人设一个visited，判断路径再设置一个visited。

## 数据结构

+ graph 记录所有通讯记录，是一个字典

  + 键是用户姓名
  + 值又是一个字典，表示这个用户对其他用户的通讯量。
    + 键是打给的人的姓名
    + 值是时间长度
+ visited 是一个字典，表示这个用户有没有被访问过

  + 键是用户姓名
  + 值是布尔变量
+ roads_visited 代表通讯录上的每一条路有没有被访问过。所以它和graph一样，只是最后的值并不是时间长度而是有没有被访问过。
+ weight是一个字典，代表每个用户的权重

  + 键是用户名
  + 值是权重
+ num，whole_weight，head，max_person_weight 表示这个团体的总人数，总权重，头头姓名和头头权重
+ ans是一个列表，存放答案。每一项是一个列表，代表一行，也就是头头姓名和团体人数。
+ 注意，只使用一个visited是不行的。
+ 因为即便通过dfs完来回溯使visited为False，也可以保证每条路都访问到，但是这样计算num就出问题了。
+ 你也可以设置两个visited都表示点有没有被访问过，其中一个回溯而另一个不回溯。算weight的时候，使用回溯的visited来判断要不要加，而算num的时候，使用不回溯的visited来确保不会重复访问。

## 算法

+ 读入数据，按照上述的数据结构存储（这种数据结构真的很方便很爽）。
+ 计算每个人的权重（这里使用了列表生成式，真的很方便很爽）
+ 对所有用户开始dfs
  + 如果这个用户已经访问过，代表是一个已有团体，直接跳过
  + 否则，说明是个新团体
  + 初始化团队人数,总重量，并假定目前的头头就是他
  + 开始dfs
    + 如果这个人未访问过，那么团队数量加一，并且把他设为访问过。
    + 对这个人的通讯录上的所有通讯记录
      + 如果通讯记录没被调查过
      + 那么总权重加上通讯时间
      + 标记路线被访问过
      + 如过通讯对象是头头，则更新头头
      + dfs通讯对象
  + 完了以后判断这个团体权重赔不配得上黑社会，并相应添加到答案中
+ 排序并输出。

##　代码

由于使用Python能AC，因此只放了Python的代码。

```Python
from collections import defaultdict


def dfs(name):
    global head, whole_weight, graph, visited, max_person_weight, num
    if not visited[name]:
        num += 1
    	visited[name] = True
    for person, time in graph[name].items():
        if not roads_visited[name][person]:
            whole_weight += time
            roads_visited[name][person] = True
            roads_visited[person][name] = True
            if weight[person] > max_person_weight:
                max_person_weight = weight[person]
                head = person
            dfs(person)


num_calls, threshold = list(map(int, input().split()))
graph = defaultdict(lambda: defaultdict(int))
visited = defaultdict(bool)
roads_visited = defaultdict(lambda: defaultdict(bool))
ans = []
for _ in range(num_calls):
    info = input().split()
    graph[info[0]][info[1]] += int(info[2])
    graph[info[1]][info[0]] += int(info[2])
weight = {i: sum(graph[i].values()) for i in graph.keys()}

for i in graph.keys():
    if not visited[i]:
        num = 0
        whole_weight = 0
        head = i
        max_person_weight = weight[i]
        dfs(i)
        if whole_weight >= threshold and num > 2:
            ans.append([head, num])

print(len(ans))
ans.sort(key=lambda x: x[0])
for i, j in ans:
    print(i, j)

```



