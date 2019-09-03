---
title: <Python><PAT> 1101 Quick Sort
date: 2019-09-03 13:00:04
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 名字叫快排，但实际上和快排关系不大
---

# 题目

There is a classical process named **partition** in the famous quick sort algorithm. In this process we typically choose one element as the pivot. Then the elements less than the pivot are moved to its left and those larger than the pivot to its right. Given *N* distinct positive integers after a run of partition, could you tell how many elements could be the selected pivot for this partition?

For example, given *N*=5 and the numbers 1, 3, 2, 4, and 5. We have:

- 1 could be the pivot since there is no element to its left and all the elements to its right are larger than it;
- 3 must not be the pivot since although all the elements to its left are smaller, the number 2 to its right is less than it as well;
- 2 must not be the pivot since although all the elements to its right are larger, the number 3 to its left is larger than it as well;
- and for the similar reason, 4 and 5 could also be the pivot.

Hence in total there are 3 pivot candidates.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer $N(10^5)$. Then the next line contains *N* distinct positive integers no larger than 109. The numbers in a line are separated by spaces.

### Output Specification:

For each test case, output in the first line the number of pivot candidates. Then in the next line print these candidates in increasing order. There must be exactly 1 space between two adjacent numbers, and no extra space at the end of each line.

### Sample Input:

```in
5
1 3 2 4 5
```

### Sample Output:

```out
3
1 4 5
```

# 题解

## 思路

+ 这道题有很多方法
+ 有一种方法是，pivot必须在它排好序的位置上
  + 那么就需要对原数排序，找两个序列下标相同的那些项
  + 但是有些数在自己应该在的位置，但是左边有比它大的，右边有比它小的
  + 那这样又要频繁使用`MIN()`函数
  + 加上排序，这样会导致复杂度比较大，不建议使用
+ 还有一种方法是运用线性扫描的思想
  + 设两个变量为左边最大和右边最小，初始化为`nums[0]`和`min(nums)`
  + 遍历num
  + 每遇到一个数
    + 如果在左边最大和右边最小之间的
      + 添加进答案
      + 同时更新左边最大为它
    + 如果是等于右边最小的
      + 右边最小等于剩下的数的右边最小
  + 这个方法前面都很美好但是最后一步又是需要频繁调用`Min()`函数，复杂度依然不小
+ 所以我推荐我的方法， 是第二种方法的改良。它的复杂度是三次O(N)，也就是O(N)
+ 首先开一个left数组，意义是，到第i个数的时候，左边所有数的最大值。
+ 遍历一遍原数组更新left
+ 开一个right数组，意义是，到第i个数的时候，右边所有数的最小值。
+ 遍历一遍原数组更新right
+ 再遍历一遍原数组，如果一个数下标为`i`，当`left[i] <= nums[i] <= right[i]`的时候，把它添加进答案里
+ 输出即可。

## 数据结构

+ left 存放每个数左边的最大值（我这里设置成了包括自身，这个随意，统一即可）
  + 下标是原数的下标
  + 值是到原数的左边所有数中的最大值
+ right 存放每个数右边的最大值（我这里设置成了包括自身，这个随意，统一即可）
  - 下标是原数的下标
  - 值是到原数的右边所有数中的最大值b
+ count 存放答案

## 算法

+ 在思路里已经有了详述。

## 代码

+ 由于使用Python3能AC，因此只放了Python3的代码。这个要看运气，大概三次里面会有一次超时，不必紧张，重复提交几次即可。

```python
n = int(input())
nums = list(map(int, input().split()))
# 找每个数左边的最大值
left = [nums[0] for _ in range(n)]
for i in range(1, n):
    left[i] = nums[i] if nums[i] > left[i - 1] else left[i - 1]
# 找每个数右边的最小值
right = [nums[-1] for _ in range(n)]
for i in range(n - 2, -1, -1):
    right[i] = nums[i] if nums[i] < right[i + 1] else right[i + 1]
# 输出
count = [j for i, j in enumerate(nums) if left[i] <= j <= right[i]]
print(len(count))
print(" ".join(list(map(str, count))))
```

