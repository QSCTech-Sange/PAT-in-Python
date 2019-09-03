---
title: 1089 Insert or Merge
date: 2019-09-03 11:36:04
tags:
---

# 题目

According to Wikipedia:

**Insertion sort** iterates, consuming one input element each repetition, and growing a sorted output list. Each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there. It repeats until no input elements remain.

**Merge sort** works as follows: Divide the unsorted list into N sublists, each containing 1 element (a list of 1 element is considered sorted). Then repeatedly merge two adjacent sublists to produce new sorted sublists until there is only 1 sublist remaining.

Now given the initial sequence of integers, together with a sequence which is a result of several iterations of some sorting method, can you tell which sorting method we are using?

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤100). Then in the next line, *N* integers are given as the initial sequence. The last line contains the partially sorted sequence of the *N* numbers. It is assumed that the target sequence is always ascending. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in the first line either "Insertion Sort" or "Merge Sort" to indicate the method used to obtain the partial result. Then run this method for one more iteration and output in the second line the resuling sequence. It is guaranteed that the answer is unique for each test case. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

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

```in
10
3 1 2 8 7 5 9 4 0 6
1 3 2 8 5 7 4 9 0 6
```

### Sample Output 2:

```out
Merge Sort
1 2 3 8 4 5 7 9 0 6
```

# 题解

## 思路

+ 题目的意思是让你判断一个排了一半的序列用的是插入排序还是归并排序，并输出下一轮排序结果
+ 首先要抓插入排序和归并排序的特点，基本排序算法的思想要牢记啊！
+ 插入排序的特点是，前面几位都是有序的，后面都是和原序列一样的。
+ 所以我们可以找到第一个不有序的数，然后比较其后的数和原序列是不是相等来判断是不是插入排序
+ 如果是插入排序的话
  + 直接将前面排序好的序列加上那个不有序的数一起排序
  + 再和后面的原数合并
  + 输出
+ 如果是归并排序的话
  + 要先判断归并到了哪一层
    + 预设排了第2层
    + 如果在所有的数按照层数分割（例如一开始就是两两分割），都是有序的话，那么进入下一层，即层数乘以2
  + 否则，再按照下一层排序
    + 同样是按找层数分割，最后合并。
  + 输出即可

## 数据结构

+ original 原来的序列
+ part_sorted 排了一半的序列
+ index 找到第一个不按照从小到大排序的数
+ merge_levele 为排序的层数

## 算法

+ 在思路里已经有了很详尽的叙述。

## 代码

+ 由于使用Python可以AC，因此只放了Python的解。

```python
num = int(input())
original = list(map(int, input().split()))
part_sorted = list(map(int, input().split()))
for i in range(1,len(part_sorted)):             # 找第一个不按照从小到大排序的数
    if part_sorted[i] < part_sorted[i - 1]:
        index = i
        break
if original[index:] == part_sorted[index:]:		# 插入排序
    print("Insertion Sort")
    part_sorted = sorted(part_sorted[:index + 1]) + part_sorted[index + 1:]
    print(" ".join(list(map(str, part_sorted))))
else:											# 归并排序
    print("Merge Sort")
    merge_level = 2
    while all([part_sorted[i:i + merge_level] == sorted(part_sorted[i:i + merge_level]) for i in
               range(0, len(part_sorted), merge_level)]):
        merge_level *= 2
    ans = []
    for i in range(0, len(part_sorted), merge_level):
        ans += sorted(part_sorted[i:i + merge_level])
    print(" ".join(list(map(str, ans))))

```

