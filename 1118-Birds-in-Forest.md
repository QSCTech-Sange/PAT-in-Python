---
title: <Python><PAT> 1118 Birds in Forest
date: 2019-09-04 19:40:50
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 并查集的运用
---

# 题目

Some scientists took pictures of thousands of birds in a forest. Assume that all the birds appear in the same picture belong to the same tree. You are supposed to help the scientists to count the maximum number of trees in the forest, and for any pair of birds, tell if they are on the same tree.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive number $N (≤10^4)$ which is the number of pictures. Then *N* lines follow, each describes a picture in the format:

$K \;B_1\; B_2 ... B_K$

where *K* is the number of birds in this picture, and $B_i$'s are the indices of birds. It is guaranteed that the birds in all the pictures are numbered continuously from 1 to some number that is no more than $10^4$.

After the pictures there is a positive number $Q (≤10^4)$ which is the number of queries. Then *Q* lines follow, each contains the indices of two birds.

### Output Specification:

For each test case, first output in a line the maximum possible number of trees and the number of birds. Then for each query, print in a line `Yes` if the two birds belong to the same tree, or `No` if not.

### Sample Input:

```in
4
3 10 1 2
2 3 4
4 1 5 7 8
3 9 6 4
2
10 5
3 7
```

### Sample Output:

```out
2 10
Yes
No
```

# 题解

## 思路

+ 理解了题目意思就好做了
+ 意思是给定N行
+ 每行给定N只鸟，这几只鸟在一棵数上
+ 如果后面的行出现了前面相同的鸟，那么这些所有的鸟都在一棵树上
+ 很明显使用并查集即可

## 数据结构

+ father 是一个并查集，
  + 下标是鸟的id
  + 值是这只鸟的上级
+ birds 是一次输入的一行中的所以鸟
+ set_bird 存放所有鸟的id

## 算法

+ 对每一行的输入
  + 每一只鸟都和第一只鸟作并查集的合并，即一只鸟的根的父亲是另一只鸟的根。
  + set_bird集合里添加这只鸟
+ 遍历set_bird，当遇到了一只鸟的上级就是自己的时候，说明这只鸟是老大，统计所有老大。
+ 输出老大的数量和总共的鸟的数量
+ 读入查询的鸟
+ 当两只鸟都在set_bird里同时它们的老大相同时，输出Yes，否则输出No

## 代码

+ 这道题测试点有一个没有严格按照题目给出，所以既写了Python解又写了C++解。

### Python

```python
n = int(input())
father = [i for i in range(10001)]
set_bird = set()

def findFather(bird):
    if father[bird] != bird:
        father[bird] = findFather(father[bird])
    return father[bird]


for _ in range(n):
    birds = list(map(int, input().split()[1:]))
    for bird in birds:
        father[findFather(bird)] = findFather(birds[0])
        set_bird.add(bird)

tree = 0
for bird in set_bird:
    if father[bird] == bird:
        tree += 1
print(tree, len(set_bird))

que = int(input())
for i in range(que):
    a, b = list(map(int, input().split()))
    if a in set_bird and b in set_bird and findFather(a) == findFather(b):
        print("Yes")
    else:
        print("No")

```

### C++

```c++
#include <iostream>
#include <unordered_set>
#include <vector>

using namespace std;

int father[10001];

int findFather(int bird) {
    if (father[bird] != bird)
        father[bird] = findFather(father[bird]);
    return father[bird];
}


int main() {
    for (int i = 0; i < 10001; i++)
        father[i] = i;
    int n;
    cin >> n;
    unordered_set<int> set_bird;
    for (int i = 0; i < n; i++) {
        int num;
        cin >> num;
        vector<int> temp;
        for (int j = 0; j < num; j++) {
            int bbird;
            cin >> bbird;
            set_bird.insert(bbird);
            temp.push_back(bbird);
            if (j != 0)
                father[findFather(bbird)] = findFather(temp[0]);
        }
    }
    
    int tree = 0;
    for (auto it:set_bird) {
        if (father[it] == it)
            tree += 1;
    }
    cout << tree << " "<<set_bird.size() << endl;
    
    int que;
    cin >> que;
    for (int i = 0; i < que; i++) {
        int a, b;
        cin >> a >> b;
        if (set_bird.count(a) == 1 and set_bird.count(b) == 1 and findFather(a) == findFather(b))
            printf("Yes\n");
        else
            printf("No\n");
    }
}
```

