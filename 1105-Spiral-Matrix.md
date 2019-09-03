---
title: <Python><PAT> 1105 Spiral Matrix
date: 2019-09-03 15:48:52
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 这道题让你掌握蛇皮走位
---

# 题目

This time your job is to fill a sequence of *N* positive integers into a **spiral matrix** in non-increasing order. A spiral matrix is filled in from the first element at the upper-left corner, then move in a clockwise spiral. The matrix has *m* rows and *n* columns, where *m* and *n* satisfy the following: *m*×*n* must be equal to *N*; *m*≥*n*; and *m*−*n* is the minimum of all the possible values.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N*. Then the next line contains *N* positive integers to be filled into the spiral matrix. All the numbers are no more than 104. The numbers in a line are separated by spaces.

### Output Specification:

For each test case, output the resulting matrix in *m* lines, each contains *n* numbers. There must be exactly 1 space between two adjacent numbers, and no extra space at the end of each line.

### Sample Input:

```in
12
37 76 20 98 76 42 53 95 60 81 58 93
```

### Sample Output:

```out
98 95 93
42 37 81
53 20 76
58 60 76
```

# 题解

## 思路

+ 题目的意思是排序然后螺旋输出，即按照那个范例。
+ 我们首先要找到行和列，题目的要求是行乘以列等于N，同时行和列尽可能接近。
+ 这里将coloumns（小的那个） 先等于原数开根号
+ 然后当N不能被columns整除的时候，column -= 1
+ 最后row = N % column，这样就求出来了columns和rows，为列数和行数
+ 然后先把原数组从大到小排一下。
+ 接着开始蛇皮排序，在算法里详解。

## 数据结构

+ nums 是原数列，我们对其逆序排序。
+ rows和columns是行数和列数，总共的。
+ matrix即输出的矩阵，有rows行和columns列
+ row和col是目前位置的行坐标和列坐标。
+ left,right,up,down是目前的左，右，上，下边界。
+ i 记录已经录入了多少数。

## 算法

+ 如何进行蛇皮走位呢？
+ 我们需要
+ row和col是目前位置的行坐标和列坐标。
+ left,right,up,down是目前的左，右，上，下边界。
+ 一开始row和col是0,0,而左边界和上边界也是0,但是右下边界分别是columns - 1， rows - 1
+ 我们开始循环，每次循环分为四个方向，即右下左上
  + 每个方向上
  + 依照方向更新row或col
  + 更新边界
  + 将row和col定位到下一个该填上空位的地方
+ 直到所有的数被填满，循环结束。
+ 输出即可。

## 代码

+ 由于使用Python能AC，因此只放了Python的题解。

```python
n = int(input())
nums = sorted(list(map(int, input().split())), reverse=True)
columns = int(n ** 0.5)
while n % columns != 0:
    columns -= 1
rows = n // columns

matrix = [[None for _ in range(columns)] for _ in range(rows)]

row, col = 0, 0
left, right, up, down = 0, columns - 1, 0, rows - 1
i = 0
while i < columns * rows:
    while col <= right and i < columns * rows:
        matrix[row][col] = nums[i]
        col += 1
        i += 1
    up += 1
    col -= 1
    row += 1
    while row <= down and i < columns * rows:
        matrix[row][col] = nums[i]
        row += 1
        i += 1
    right -= 1
    row -= 1
    col -= 1
    while col >= left and i < columns * rows:
        matrix[row][col] = nums[i]
        col -= 1
        i += 1
    down -= 1
    col += 1
    row -= 1
    while row >= up and i < columns * rows:
        matrix[row][col] = nums[i]
        row -= 1
        i += 1
    left += 1
    row += 1
    col += 1
    
for i in matrix:
    print(" ".join(list(map(str, i))))

```

