---
title: <Python><PAT> 1109 Group Photo
date: 2019-09-03 23:03:53
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 在？拍个照？
---

# 题目

Formation is very important when taking a group photo. Given the rules of forming *K* rows with *N* people as the following:

- The number of people in each row must be *N*/*K* (round down to the nearest integer), with all the extra people (if any) standing in the last row;
- All the people in the rear row must be no shorter than anyone standing in the front rows;
- In each row, the tallest one stands at the central position (which is defined to be the position (*m*/2+1), where *m* is the total number of people in that row, and the division result must be rounded down to the nearest integer);
- In each row, other people must enter the row in non-increasing order of their heights, alternately taking their positions first to the right and then to the left of the tallest one (For example, given five people with their heights 190, 188, 186, 175, and 170, the final formation would be 175, 188, 190, 186, and 170. Here we assume that you are facing the group so your left-hand side is the right-hand side of the one at the central position.);
- When there are many people having the same height, they must be ordered in alphabetical (increasing) order of their names, and it is guaranteed that there is no duplication of names.

Now given the information of a group of people, you are supposed to write a program to output their formation.

### Input Specification:

Each input file contains one test case. For each test case, the first line contains two positive integers $N(<10^4)$, the total number of people, and *K* (≤10), the total number of rows. Then *N*lines follow, each gives the name of a person (no more than 8 English letters without space) and his/her height (an integer in [30, 300]).

### Output Specification:

For each case, print the formation -- that is, print the names of people in *K* lines. The names must be separated by exactly one space, but there must be no extra space at the end of each line. Note: since you are facing the group, people in the rear rows must be printed above the people in the front rows.

### Sample Input:

```in
10 3
Tom 188
Mike 170
Eva 168
Tim 160
Joe 190
Ann 168
Bob 175
Nick 186
Amy 160
John 159
```

### Sample Output:

```out
Bob Tom Joe Nick
Ann Mike Eva
Tim Amy John
```

# 题解

## 思路

+ 这道题是一堆人要照相，让你根据他们的身高来排序
+ 题目里告诉了一堆排序的原则，最后告诉你你是面对它们的，所以左右前后要反一反，很无语。
+ 所以我直接告诉你反转后的规则
+ 首先所有人按照从高到矮，和姓名从小到大排序
+ 它给定了人数和行数，首先每行的人数是人数//行数，有余数的，补在第一行里
+ 按前面的排序从第一行一行一行往后填
+ 在一行中，最高的排在num//2的位置（题目里不用加一，反转后就要加一。即中间的位置，如果是偶数，排在靠右的。因为num是从1计的而下标是从0计的，所以这样就是加过1的）
+ 其他人按照原来的排序，先排在最高的左边，然后右边，然后左边……
+ 具体可以参考题目里的样例
+ 只要理解了题意，我们就可以仿照题目的顺序来排了。
+ 首先排序，然后按照一行一行来处理并添加到答案里即可。

## 数据结构

+ num 总人数
+ rows 总行数
+ people 是一个列表，存放所有的人
  + 一个人是一个列表，包含两项
  + 第一项是名字
  + 第二项是身高
+ count记录我们已经排了多少人了
+ ans存放一个矩阵，保存要输出的排序后的格式
+ index 记录在一行中，我们已经排了多少人了
+ left 记录下一个要填充的人在左边的哪一个下标
+ right 记录下一个要填充的人在右边的哪一个下标
+ round 是这一行的人
+ temp 是这一行的位置

## 算法

+ 初始化数据结构，让所有人根据题目里的要求排序
+ 记录count为已经固定好位置的人数，一开始为0,当count不等于总人数的时候，进入下列循环
+ 一次循环表示分配一行的人
  + 首先找到要排的这一行的人在总人数的位置，即`count:count + num // rows`
  + 但是要注意是首行的话要把余数添加进去
  + 然后给count增加这一行的人数
  + 给这一行分配位置，开辟了一个this_row列表
  + 我们的目的就是把这一行的人添加到this_row合适的位置
  + 找到left和right，即下一个要在左右添加的时候，它们的下标
  + 先把中心最高的人填满，设index为1，即这一行已经填了一个
  + 当index < len(round) 即这一行还没填满的时候
  + 按照先左后右的原则填补人进去
  + 同时更新index和left或right
  + 最后把这一行推进答案里
+ 输出即可

## 代码

```python
num, rows = list(map(int, input().split()))
people = []
for _ in range(num):
    name, height = input().split()
    people.append([name, int(height)])
people.sort(key=lambda x: (-x[1], x[0]))
count = 0
ans = []
while count < num:
    # 找到这一行的人的范围
    round = people[count:count + num // rows + num % rows] if count == 0 else people[count:count + num // rows]
    count += len(round)
    this_row = [None for i in range(len(round))]
    left = len(round) // 2 - 1
    right = len(round) // 2 + 1
    # 填补中心的位置
    this_row[left + 1] = round[0]
    # 填补左右的位置
    index = 1
    while index < len(round):
        this_row[left] = round[index]
        index += 1
        left -= 1
        if index < len(round):
            this_row[right] = round[index]
            index += 1
            right += 1
    ans.append([i[0] for i in this_row])

for i in ans:
    print(" ".join(i))

```

