---
title: <Python><PAT> 1148 Werewolf - Simple Version
date: 2019-09-06 21:04:36
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 趣味狼人杀
---

# 题目

Werewolf（狼人杀） is a game in which the players are partitioned into two parties: the werewolves and the human beings. Suppose that in a game,

- player #1 said: "Player #2 is a werewolf.";
- player #2 said: "Player #3 is a human.";
- player #3 said: "Player #4 is a werewolf.";
- player #4 said: "Player #5 is a human."; and
- player #5 said: "Player #4 is a human.".

Given that there were 2 werewolves among them, at least one but not all the werewolves were lying, and there were exactly 2 liars. Can you point out the werewolves?

Now you are asked to solve a harder version of this problem: given that there were *N* players, with 2 werewolves among them, at least one but not all the werewolves were lying, and there were exactly 2 liars. You are supposed to point out the werewolves.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (5≤*N*≤100). Then *N* lines follow and the *i*-th line gives the statement of the *i*-th player (1≤*i*≤*N*), which is represented by the index of the player with a positive sign for a human and a negative sign for a werewolf.

### Output Specification:

If a solution exists, print in a line in ascending order the indices of the two werewolves. The numbers must be separated by exactly one space with no extra spaces at the beginning or the end of the line. If there are more than one solution, you must output the smallest solution sequence -- that is, for two sequences *A*=*a*[1],...,*a*[*M*] and *B*=*b*[1],...,*b*[*M*], if there exists 0≤*k*<*M* such that *a*[*i*]=*b*[*i*] (*i*≤*k*) and *a*[*k*+1]<*b*[*k*+1], then *A* is said to be smaller than *B*. In case there is no solution, simply print `No Solution`.

### Sample Input 1:

```in
5
-2
+3
-4
+5
+4
```

### Sample Output 1:

```out
1 4
```

### Sample Input 2:

```in
6
+6
+3
+1
-5
-2
+4
```

### Sample Output 2 (the solution is not unique):

```out
1 5
```

### Sample Input 3:

```in
5
-2
-3
-4
-5
-1
```

### Sample Output 3:

```out
No Solution
```

# 题解

## 思路

+ 挺有趣的一道题
+ 有N个人，每个人说一句话，认为一个人是狼人还是村民
+ 实际上，总共有两个狼人，且正好有一个狼人和一个村民在说谎
+ 题目让我们输出从小到大序号的两个狼人
+ 初看无从下手，其实就是暴力遍历
+ 总体方针就是
  + 假定两个狼人是狼人
  + 那么就可以判断其他人说的是否是实话
  + 那么就可以统计说谎的人
  + 那么就可以判断是不是正好两人，是不是正好一个狼人和一个村民
+ 而我们要从小到大输出，所以我们可以先假定第一狼的下标遍历从1到num，而第二狼从第一狼下标+1到num+1

## 数据结构

+ guess读取所有人的猜测，以整数形式放置
  + 下标是id
  + 值是猜测
+ first 是第一狼的下标
+ second 是第二狼的下标
+ identity 记录每个人是村民还是狼
  + 下标是id
  + 值 当狼时为-1,村民时为1
+ liars 记录所有说谎的人的id

## 算法

+ 读取所有人的猜测
+ 第一狼开始遍历
  + 第二狼开始遍历
    + 先初始化identity，设置为这两只狼为-1,其他为1
    + 遍历每一个人的话，判断他说的是不是实话，放到liars里面
      + 一个人的话/一个人的话的绝对值 表示他对这个人身份的猜测
      + identity[这个人的话的绝对值] 表示这个人的真实身份
      + 两者不同即说谎
    + 判断是不是总共两个人说谎且正好有一只狼在说谎
    + 说谎了就输出
+ 一直没输出过打印无解

## 代码

+ 由于使用Python能够AC，因此只放了Python的题解。

```python
num = int(input())
guess = [0 for i in range(num + 1)]
for i in range(num):
    guess[i + 1] = int(input())


def judge():
    for first in range(1, num):
        for second in range(first + 1, num + 1):
            identity = [1 for _ in range(num + 1)]
            identity[first] = -1
            identity[second] = -1
            liars = [i for i in range(1,num+1) if identity[abs(guess[i])] != guess[i]/abs(guess[i])]
            if len(liars) == 2 and (first in liars or second in liars) and liars != [first, second]:
                print(first, second)
                return 0
    else:
        print("No Solution")


judge()

```

