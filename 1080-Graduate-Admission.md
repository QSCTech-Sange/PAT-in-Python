---
title: <Python><PAT> 1080 Graduate Admission
date: 2019-09-01 18:32:47
tags: 
- PAT
- 题解
- 排序
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 使用Python爽爆的排序。这个题正好卡在了Python的时间线上，多运行几次，有时候能AC,有时候会有一个点超时。
---

# 题目

It is said that in 2011, there are about 100 graduate schools ready to proceed over 40,000 applications in Zhejiang Province. It would help a lot if you could write a program to automate the admission procedure.

Each applicant will have to provide two grades: the national entrance exam grade $G_E$, and the interview grade $G_I$. The final grade of an applicant is $(G_E+G_I)/2$. The admission rules are:

- The applicants are ranked according to their final grades, and will be admitted one by one from the top of the rank list.
- If there is a tied final grade, the applicants will be ranked according to their national entrance exam grade $G_E$. If still tied, their ranks must be the same.
- Each applicant may have $K$ choices and the admission will be done according to his/her choices: if according to the rank list, it is one's turn to be admitted; and if the quota of one's most preferred shcool is not exceeded, then one will be admitted to this school, or one's other choices will be considered one by one in order. If one gets rejected by all of preferred schools, then this unfortunate applicant will be rejected.
- If there is a tied rank, and if the corresponding applicants are applying to the same school, then that school must admit all the applicants with the same rank, **even if its quota will be exceeded**.

### Input Specification:

Each input file contains one test case.

Each case starts with a line containing three positive integers: *N* (≤40,000), the total number of applicants; *M* (≤100), the total number of graduate schools; and *K* (≤5), the number of choices an applicant may have.

In the next line, separated by a space, there are *M* positive integers. The *i*-th integer is the quota of the *i*-th graduate school respectively.

Then *N* lines follow, each contains 2+*K* integers separated by a space. The first 2 integers are the applicant's $G_E$ and ,$G_I$ respectively. The next *K* integers represent the preferred schools. For the sake of simplicity, we assume that the schools are numbered from 0 to *M*−1, and the applicants are numbered from 0 to *N*−1.

### Output Specification:

For each test case you should output the admission results for all the graduate schools. The results of each school must occupy a line, which contains the applicants' numbers that school admits. The numbers must be in increasing order and be separated by a space. There must be no extra space at the end of each line. If no applicant is admitted by a school, you must output an empty line correspondingly.

### Sample Input:

```in
11 6 3
2 1 2 2 2 3
100 100 0 1 2
60 60 2 3 5
100 90 0 3 4
90 100 1 2 0
90 90 5 1 3
80 90 1 0 2
80 80 0 1 2
80 80 0 1 2
80 70 1 3 2
70 80 1 2 3
100 100 0 2 4
```

### Sample Output:

```out
0 10
3
5 6 7
2 8

1 4
```

# 题解

## 思路

+ 看起来题目挺庞大，但是挺好理解的
+ 就是给你考生排名和志愿，让你最后输出每个学校录取了哪些人
+ 因为和真实流程非常接近，所以觉得题目并不抽象，很好理解
+ 给定学生的高考成绩和面试成绩，按照总成绩排名，相同的按照高考成绩排名，都相同的，两者地位相同
+ 按照排名，遍历学生。
+ 遍历学生的每个志愿，如果学校没有招满，那么就招进来他。
+ 如果两个学生成绩一模一样且学校招得只剩一个名额了，两个都要进去。即不退档。
+ 我们模拟这个来就行了。作为30分的大题确实简单了一点。

## 数据结构

+ applicants  是一个列表，存放所有的applicant，所有的考生
+ applicant 是一个考生，每一个考生用一个列表表示
  + 第一项是高考成绩
  + 第二项是面试成绩
  + 第三项是他的志愿们
  + 第四项是他的id
+ schools 是一个列表，记录了每个学校录取了那些人
  + 下标是学校的ID
  + 值是一个列表，存放着录取了的所有人。每一个人是一个上文所述的applicant数据类型
+ quota 是一个列表，表示每个学校的招生名额
  + 下标是学校id
  + 值是招生名额

## 算法

+ 读取数据并初始化数据结构
+ 对applicants排序，按照总成绩优先，高考成绩其次的原则
+ 在applicants里遍历每一个考生
  + 遍历每一个考生的所有志愿
    + 如果这个志愿的学校没有招满
    + 或者已经招满了，但是这个考生和招的最后一名所有成绩都相同
    + 那么把这个考生录进去
    + break这个循环
+ 最后输出录取信息，记得对每个学校的录取考生排序。

## 代码

+ 使用Python的话，多提交几次就能过。因为时间点是250ms，Python的运行时间在230~270ms之间波动。所以这道题给我们一个经验就是，当我们有运行超时的情况发生的时候，可以先重复提交几次，如果确认了还是超时，那么再转战C++也不迟。

```python
num_applicant, num_school, num_choice = list(map(int, input().split()))
schools = [[] for _ in range(num_school)]
quota = list(map(int, input().split()))
applicants = [[] for _ in range(num_applicant)]
for i in range(num_applicant):
    exam, interview, *choices = list(map(int, input().split()))
    applicants[i] = [exam, interview, choices, i]
# 对考生排序
applicants.sort(key=lambda x: (x[0] + x[1], x[0]), reverse=True)
# 遍历每个考生的每个志愿
for applicant in applicants:
    for choice in applicant[2]:
        if len(schools[choice]) < quota[choice] or (
                schools[choice][-1][0] == applicant[0] and schools[choice][-1][1] == applicant[1]):
            schools[choice].append(applicant)
            break
# 输出
for school in schools:
    print(" ".join(list(map(str, sorted([i[3] for i in school])))))

```

