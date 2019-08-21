---
title: <Python><PAT> 1006 Sign In and Sign Out
date: 2019-08-19 15:06:11
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: Easy题，使用Python处理字符串是真的爽爆啊！
---

# 题目

At the beginning of every day, the first person who signs in the computer room will unlock the door, and the last one who signs out will lock the door. Given the records of signing in's and out's, you are supposed to find the ones who have unlocked and locked the door on that day.

### Input Specification:

Each input file contains one test case. Each case contains the records for one day. The case starts with a positive integer *M*, which is the total number of records, followed by *M* lines, each in the format:

```
ID_number Sign_in_time Sign_out_time
```

where times are given in the format `HH:MM:SS`, and `ID_number` is a string with no more than 15 characters.

### Output Specification:

For each test case, output in one line the ID numbers of the persons who have unlocked and locked the door on that day. The two ID numbers must be separated by one space.

Note: It is guaranteed that the records are consistent. That is, the sign in time must be earlier than the sign out time for each person, and there are no two persons sign in or out at the same moment.

### Sample Input:

```in
3
CS301111 15:30:28 17:00:10
SC3021234 08:00:00 11:25:25
CS301133 21:45:00 21:58:40
```

### Sample Output:

```out
SC3021234 CS301133
```



# 题解

## 思路

+ 搞一个最早到时间为9999999和最晚出来时间为0
+ 每次读取一个学生就看情况更新最早到时间和最晚到时间，并记录id
+ 时间处理成秒
+ 完事儿

## 数据结构

+ early, early_id 为最早到的时间和id
+ late, late_id 为最晚到的时间和id

## 算法

+ 还能有啥算法，遍历输入并更新呗

## 代码

因为使用Python3能AC，因此使用Python来写代码。主要是Python处理字符串方便，所以这题显得简单。

```python
num_stu = int(input())
early, late = 9999999999, 0
for _ in range(num_stu):
    id, come, go = input().split()
    # 将come和go转换成秒
    come = come.split(":")
    come = int(come[0]) * 3600 + int(come[1]) * 60 + int(come[2])
    go = go.split(":")
    go = int(go[0]) * 3600 + int(go[1]) * 60 + int(go[2])
    # 比较是否需要更新
    if come < early:
        early = come
        early_id = id
    if go > late:
        late = go
        late_id = id

print(early_id, late_id)
```

