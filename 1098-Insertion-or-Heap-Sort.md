---
title: <Python><PAT> 1098 Insertion or Heap Sort
date: 2019-08-18 17:23:21
tags: 
- PAT
- 题解
- DFS
- Dijkstra
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 还是有难度的，尤其当你忘了什么是堆排序的时候
---

# 题目

According to Wikipedia:

**Insertion sort** iterates, consuming one input element each repetition, and growing a sorted output list. Each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there. It repeats until no input elements remain.

**Heap sort** divides its input into a sorted and an unsorted region, and it iteratively shrinks the unsorted region by extracting the largest element and moving that to the sorted region. it involves the use of a heap data structure rather than a linear-time search to find the maximum.

Now given the initial sequence of integers, together with a sequence which is a result of several iterations of some sorting method, can you tell which sorting method we are using?

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤100). Then in the next line, *N* integers are given as the initial sequence. The last line contains the partially sorted sequence of the *N* numbers. It is assumed that the target sequence is always ascending. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in the first line either "Insertion Sort" or "Heap Sort" to indicate the method used to obtain the partial result. Then run this method for one more iteration and output in the second line the resulting sequence. It is guaranteed that the answer is unique for each test case. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

### Sample Input 1:

```in
10
3 1 2 8 7 5 9 4 6 0
1 2 3 7 8 5 9 4 6 0
```

### Sample Output 1:

```out
Insertion Sort
1 2 3 5 7 8 9 4 6 0
```

### Sample Input 2:

```
10
3 1 2 8 7 5 9 4 6 0
6 4 5 1 0 3 2 7 8 9
```

### Sample Output 2:

```
Heap Sort
5 4 3 1 0 2 6 7 8 9
```

# 题解

## 思路

+ 首先需要理解题意
  + 题目意思是给两个数列，第一个数列是原数列，第二个是排序了一半的序列
  + 让你判断第二个数列是用插入排序的还是堆排序的
  + 并且给出再一轮后的排序

+ 然后要抓两种排序的不同点
  + 插入排序的特点是，前几个都是排序的，后面和原序列是一样的
  + 堆排序的特点是，后面几个都是排序的，但是前面的是乱的
+ 我们可以利用插入排序的特点，找到第一个**不**从小到大排列的数，比较这个数后面的所有数和原数列是否是一样的，一样的话就说明是插入排序，否则是堆排序
+ 因为插入排序一次会把下一个数移动到前面合适的位置，所以下一次排序就只要将前面n个排好的数和第n+1个数排列一下就行
+ 而堆排序就只能下滤，如果忘了堆排序是怎么排的就gg了。

> 简单补充一下堆排序，如果是从小到大排序的话，要先建一个大顶堆。从根部也就是第一个下标是最大的。每一轮（也符合题目说的一轮），将根和尾调换，然后将根下滤。最后的节点不再被视为根

## 代码

这道题光用Python就能ac，因此只写了Python代码。

### Python3

```python
# 数据读入
n = int(input())
original = [-9999] + list(map(int, input().split()))
half_sorted = [-9999] + list(map(int, input().split()))

# temp找到第一个非从小到大排序的下标
temp = 2
while temp <= n and half_sorted[temp - 1] <= half_sorted[temp]:
    temp += 1

# 如果temp之后两个数组相同，那么这就是插入排序。将半排序后的数组再往后排一位
if original[temp:] == half_sorted[temp:]:
    print("Insertion Sort")
    half_sorted = sorted(half_sorted[:temp + 1]) + half_sorted[temp + 1:]

# 否则，就是堆排
else:
    print("Heap Sort")
    # temp变量从后往前找到第一个比第一项少的数（即还没排的数），根据堆排序的性质，第一项为还未排序的最大值
    temp = n
    while temp > 2 and half_sorted[temp] >= half_sorted[1]:
        temp -= 1
    # 将第一项和temp项交换，这样第一项就是更小的了，temp项的位置就固定了
    half_sorted[1], half_sorted[temp] = half_sorted[temp], half_sorted[1]
    # 现在的第一项要开始下滤，i是j的父亲节点索引,j是i的左孩子
    i, j = 1, 2
    while j <= temp - 1:
        # 判断j要不要变成右孩子，下滤只和更大的那个交换
        if j + 1 <= temp - 1 and half_sorted[j] < half_sorted[j + 1]:
            j = j + 1
        # 交换并更新i和j，相当于下滤
        half_sorted[i], half_sorted[j] = half_sorted[j], half_sorted[i]
        i, j = j, j * 2

# 输出排序好的下一项
print(" ".join(map(str, half_sorted[1:])))

```

