---
title: <Python><PAT> 1129 Recommendation System
date: 2019-09-05 14:47:48
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 虽然叫推荐系统，但是和人工智能里的推荐系统相去甚远。
---

# 题目

Recommendation system predicts the preference that a user would give to an item. Now you are asked to program a very simple recommendation system that rates the user's preference by the number of times that an item has been accessed by this user.

### Input Specification:

Each input file contains one test case. For each test case, the first line contains two positive integers: N (≤ 50,000), the total number of queries, and K (≤ 10), the maximum number of recommendations the system must show to the user. Then given in the second line are the indices of items that the user is accessing -- for the sake of simplicity, all the items are indexed from 1 to N. All the numbers in a line are separated by a space.

### Output Specification:

For each case, process the queries one by one. Output the recommendations for each query in a line in the format:

```
query: rec[1] rec[2] ... rec[K]
```

where `query` is the item that the user is accessing, and `rec[i]` (`i`=1, ... K) is the `i`-th item that the system recommends to the user. The first K items that have been accessed most frequently are supposed to be recommended in non-increasing order of their frequencies. If there is a tie, the items will be ordered by their indices in increasing order.

Note: there is no output for the first item since it is impossible to give any recommendation at the time. It is guaranteed to have the output for at least one query.

### Sample Input:

```in
12 3
3 5 7 5 5 3 2 1 8 3 8 12
```

### Sample Output:

```out
5: 3
7: 3 5
5: 3 5 7
5: 5 3 7
3: 5 3 7
2: 5 3 7
1: 5 3 2
8: 5 3 1
3: 5 3 1
8: 3 5 1
12: 3 5 8
```

# 题解

## 思路

+ 题目意思有点抽象
+ 是给你一个序列，表示从前往后出现的商品id
+ 对应每一个id，你要输出在其之前出现次数最多的Ｋ个商品id
+ 比如`3,5,5,2`，对应5就输出３，对应第二个5输出5,3，对应最后一个2输出5,3
+  一开始想到用Counter做，配合以most_common，轻松简单且绝配。
+ 然而出现了一个小问题，就是所有id要从小到达输出。如果用most_common(K)的话，我们无法按照id从小到大输出。这样子的话就得先排序，造成复杂度上升。
+ 更好的办法是使用c++里的set，它可以保证每次插入的时候都是有序的。它和哈希集一样用，只是有序的。
+ 方法是建立两个重要的数组
+ count 记录每个数出现的次数
  + 下标是数
  + 值是出现次数
+ happened 记录出现x次数的有哪些值
  + 下标是出现的次数
  + 值是一个set，代表出现这些次数的数们
+ 每次输出的时候，就从happened里最大下标的set里找，将set里全部输出，一直输出K个，就可以了。
+ 同时更新count 和 happened

## 数据结构

- count 记录每个数出现的次数
  - 下标是数
  - 值是出现次数
- happened 记录出现x次数的有哪些值
  - 下标是出现的次数
  - 值是一个set，代表出现这些次数的数们
- max_happen 最多出现次数
- index 记录输出了几个数

## 算法

+ 读入数据，初始化count和happened
+ 先记录第一个数，即happend[1]添加这个数，count[这个数] = 1，记录max_happen为1
+ 从第二个数开始遍历。
  + 先设index为0
  + 从max_happen层级开始，找happened里最上层级的所有数
    + 输出这些数
    + 进入更小的出现次数的那一层
    + 当index > K 或者层级小于等于0 的时候结束输出
  + 更新count，使这个数的出现次数加一
  + 更新max_happen，如果能更新的话
  + 更新happend
    + 新的count里，添加这个数
    + 老的count里，删除这个数

## 代码

+ 这道题由于Python没有像c++里的set而只有哈希集，所以得多一次排序，因此远没有C++省时间。C++里的set不是哈希集，它能够保证每次插入后，set的内容始终是有序的。而同时删除和插入的时间也能控制在O(logn)。

### Python Counter

```python
from collections import Counter

num, K = list(map(int, input().split()))
products = list(map(int, input().split()))

c = Counter([products[0]])
for i in range(1, num):
    print(products[i], end=": ")
    appeared = [i[0] for i in sorted(c.most_common(), key=lambda x: (-x[1], x[0]))[:K]]
    print(" ".join(list(map(str, appeared))))
    c.update([products[i]])
```



### Python 哈希集

```python
num, K = list(map(int, input().split()))
products = list(map(int, input().split()))

happended = [set() for i in range(num + 1)]
count = [0 for _ in range(num + 1)]

count[products[0]] = 1
happended[1].add(products[0])
max_happen = 1

for i in range(1, num):
    product = products[i]
    print(str(product) + ":", end="")
    index = 0
    happen_time = max_happen
    while happen_time > 0:
        for j in sorted(happended[happen_time]):
            if index < K:
                print(" " + str(j),end='')
                index += 1
            else:
                break
        happen_time -= 1
    print()
    count[product] += 1
    max_happen = max(max_happen, count[product])
    happended[count[product]].add(product)
    if count[product] != 1:
        happended[count[product] - 1].remove(product)

```
### C++ set
```c++
#include <iostream>
#include <set>
#include <algorithm>
using namespace std;

int main() {
    int num, K;
    cin >> num >> K;
    int products[num];
    for (int i = 0; i < num; i++)
        cin>>products[i];
    set<int> happened[num];
    int count[num+1];
    for (int i = 0 ; i <= num;i++)
        count[i] =0 ;
    count[products[0]] = 1;
    happened[1].insert(products[0]);
    int max_happen = 1;

    for (int i = 1;i<num;i++){
        int product = products[i];
        printf("%d:",product);
        int index = 0;
        int happen_time = max_happen;
        while (happen_time > 0){
            for (auto j:happened[happen_time]){
                if (index < K){
                    printf(" %d",j);
                    index += 1;
                }else
                    break;
            }
            happen_time -= 1;
        }
        printf("\n");
        count[product] += 1;
        max_happen = max(max_happen,count[product]);
        happened[count[product]].insert(product);
        if (count[product] != 1)
            happened[count[product] - 1].erase(product);
    }
}
```

