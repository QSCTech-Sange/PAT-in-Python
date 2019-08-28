---
title: <Python><PAT> 1051 Pop Sequence
date: 2019-08-27 14:10:41
tags: 
- PAT
- 题解
- 栈
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 模拟堆栈
---

# 题目

Given a stack which can keep *M* numbers at most. Push *N* numbers in the order of 1, 2, 3, ..., *N* and pop randomly. You are supposed to tell if a given sequence of numbers is a possible pop sequence of the stack. For example, if *M* is 5 and *N* is 7, we can obtain 1, 2, 3, 4, 5, 6, 7 from the stack, but not 3, 2, 1, 7, 5, 6, 4.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 3 numbers (all no more than 1000): *M* (the maximum capacity of the stack), *N* (the length of push sequence), and *K* (the number of pop sequences to be checked). Then *K* lines follow, each contains a pop sequence of *N* numbers. All the numbers in a line are separated by a space.

### Output Specification:

For each pop sequence, print in one line "YES" if it is indeed a possible pop sequence of the stack, or "NO" if not.

### Sample Input:

```in
5 7 5
1 2 3 4 5 6 7
3 2 1 7 5 6 4
7 6 5 4 3 2 1
5 6 4 3 7 2 1
1 7 6 5 4 3 2
```

### Sample Output:

```out
YES
NO
NO
YES
NO
```

# 题解

## 思路

+ 题目的意思是给你一个容量固定的堆栈
+ 然后推入顺序是逐个推入从1到容量这样递增的数
+ 但是指不定推到哪里就pop一下，指不定推到哪里就pop一下
+ 于是给了你一个序列
+ 让你判断这个序列能不能是这样的stack所pop出来的序列。
+ 思路就是仿照着题目的意思
+ 即，每读到一个数，那么把它之前的全部推进去，然后pop它。
+ 遇到不符合要求的（超过容量的），就告诉它不是这个stack所能产生的。

## 数据结构

+ stack 是堆栈
+ capacity 是容量
+ sequence 是读取到的pop序列
+ num 是从1到容量的递增的数，每次把它推入栈中

## 算法

+ 这里使用了函数，因为遇到了不符合要求的一步return比较方便。
+ 照着题目意思，读取到了序列后，把这个序列交给函数处。
+ 首先初始化空栈和num = 1
+ 遍历序列的数，
  + 当栈是空的或者栈最后一个数不是遍历中的（必须有空的判断，因为`stk[-1]`对于空栈会报错），不断重复
    + 推入num
    + num += 1
    + 在这个过程中如果栈的长度超过了容量，直接报错返回
  + 如果好不容易栈末尾的数和序列末尾的数相同了
  + 弹出它
+ 如果前面过程都没错
+ 那么输出“YES”

## 代码

```python
def judge(sequence):
    stack = []
    num = 1
    for i in sequence:
        while len(stack) == 0 or stack[-1] != i:
            stack.append(num)
            num += 1
            if len(stack) > capacity:
                print("NO")
                return
        stack.pop()
    else:
        print("YES")


capacity, len_sequence, num_check = list(map(int, input().split()))
for _ in range(num_check):
    sequence = list(map(int, input().split()))
    judge(sequence)

```

