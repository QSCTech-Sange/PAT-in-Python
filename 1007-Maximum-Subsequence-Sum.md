---
title: <Python><PAT> 1007 Maximum Subsequence Sum
date: 2019-08-19 16:55:58
tags: 
- PAT
- 题解
- 动态规划
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: Hard题，需要用到动态规划，同时还有一些坑，不注意就有一些点过不了
---

# 题目

Given a sequence of *K* integers { *N*1, *N*2, ..., *N**K* }. A continuous subsequence is defined to be { *N**i*, *N**i*+1, ..., *N**j* } where 1≤*i*≤*j*≤*K*. The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements. For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.

### Input Specification:

Each input file contains one test case. Each case occupies two lines. The first line contains a positive integer *K* (≤10000). The second line contains *K* numbers, separated by a space.

### Output Specification:

For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line. In case that the maximum subsequence is not unique, output the one with the smallest indices *i* and *j* (as shown by the sample case). If all the *K* numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

### Sample Input:

```in
10
-10 1 2 3 4 -5 -23 3 7 -21
```

### Sample Output:

```out
10 1 4
```

# 题解

## 思路

这道题最合适的方法是使用**动态规划**的思想。

+ 动态规划的思想可以理解为每次都在更新当前状态下的最优情况
+ 一般是开一个dp数组，记录到每个下标的最优解
+ 但是这题不需要dp数组，不是一个典型的动态规划，但是思想是差不多的，你可以先开数组再尝试不用它，把它删掉，完全ok
+ 我们需要一个temp变量，记录目前的最优解（最大值）。
+ 我们遍历这个数组，同时将temp+=遍历的值。因为temp这时候是永远是大于等于0的，所以可以直接加遍历的值。
  + 当temp < 0的时候
    + 意味着前面的大于0的时代已经结束了，要开始新的一串大于0的时代。而这中间需要有负数的黑暗时代所隔开
    + 需要清空，也就是将temp_front指向下一项，同时temp = 0，为什么不直接更新front，是因为我们不确定下一个光明时代一定能比上一个光明时代更威武雄壮。
  + 当temp > 已经记录的最大值的时候
    + 更新最大值，front和end，其中front要等于temp_front

这道题有几个坑要注意一下

+ 一样大的时候，取最前面的序列
+ 全是负的时候，输出0和首尾数
+ 如果最大就是0,那么输出0 0 0（这个是隐含条件，很容易被忽略）

所以，为了满足第一条，我们需要在更新的时候，从前往后遍历的话，用大于号而不是大于等于来判断。这样就能记录最前面的序列。为了满足第二条，我们给它设置预设值是0和首和尾。这两点很容易这么想到。

但是！这样子就没法完成第三点了！因为遇到了0并不会更新。所以我们更合理的办法是，先把max预设为-1,这样子遇到0也会更新，同时最后输出的时候发现max为-1的话再把它改回0。

## 数据结构

+ nums是原列表。
+ front,end,largest 记录最优解的前后和最大和。
+ temp记录运算中的最大和,temp_front记录运算中的可能要更新的前部。

## 算法

+ 思路里说得挺详细了，我就不再赘述了。

## 代码

因为使用Python3能AC，因此只放了Python3的代码。

```python
# 输入与初始化数据结构
_ = int(input())
nums = list(map(int, input().split()))
front, end, largest = 0, len(nums) - 1, -1
temp, temp_front = 0, 0

# 动态规划
for i, j in enumerate(nums):
    temp += j
    if temp < 0:
        temp = 0
        temp_front = i + 1
    elif temp > largest:
        largest = temp
        front = temp_front
        end = i

# 输出
if largest == -1:
    largest = 0
print(largest, nums[front], nums[end])

```

