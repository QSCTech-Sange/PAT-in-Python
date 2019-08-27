---
title: <PAT> 1029 Median
date: 2019-08-24 09:22:52
tags: 
- PAT
- 题解
- 排序
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 这题不能再使用Python了，时间点卡得巨严。
---

# 题目

Given an increasing sequence S of N integers, the median is the number at the middle position. For example, the median of S1 = { 11, 12, 13, 14 } is 12, and the median of S2 = { 9, 10, 15, 16, 17 } is 15. The median of two sequences is defined to be the median of the nondecreasing sequence which contains all the elements of both sequences. For example, the median of S1 and S2 is 13.

Given two increasing sequences of integers, you are asked to find their median.

### Input Specification:

Each input file contains one test case. Each case occupies 2 lines, each gives the information of a sequence. For each sequence, the first positive integer N (≤2×105) is the size of that sequence. Then N integers follow, separated by a space. It is guaranteed that all the integers are in the range of **long int**.

### Output Specification:

For each test case you should output the median of the two given sequences in a line.

### Sample Input:

```in
4 11 12 13 14
5 9 10 15 16 17
```

### Sample Output:

```out
13
```

# 题解

## 思路

+ 题目是给定两个**递增**序列，让你找它们合并后的中位数。
+ 肯定不能全部读进来再排序，会超时间的。
+ 题目会给出两个序列的元素个数
+ 所以我们可以直到中位数是从小到大的第几个
+ 先存第一个序列，第二个看情况读
+ 给两个序列设定两个指针，上面小了移动上面的，下面小了移动下面的，直到移动次数是中位数的位置
+ 要注意处理好边界情况
+ 要用scanf做，使用cin容易超时。但是使用cout无所谓，因为反正就一行。

## 数据结构

+ num_a，num_b 分别是两个序列的元素个数
+ a 是第一个序列
+ tempB是正在读取的B序列的元素
+ ans存放答案
+ pointerA, pointerB分别是两个序列的指针
+ (num_a + num_b + 1)/2 给出中位数的从小到大的位置

## 算法

+ 思路里讲得很清楚了
+ 主要讨论边界情况
+ 什么时候要移动上面的序列？
  + 首先，上面的比下面小，这个毋庸置疑
  + 还有一点，就是下面的已经到底了，这时候只能动上面的
+ 什么时候要移动下面的序列？
  + 首先，下面的比上面小，这个毋庸置疑
  + 还有一点，就是上面的已经到底了，这时候只能移动下面的
+ 可以把这些合并到一起，做最初的判断。

## 代码

只使用C++。因为Python一个点也过不了。

```c++
#include <iostream>

using namespace std;

int main() {
    
    // 信息读入
    int num_a, num_b;
    scanf("%d",&num_a);
    int a[num_a];
    for (int i = 0; i < num_a; i++)
        scanf("%d",&a[i]);
    scanf("%d",&num_b);
    int tempB;
    scanf("%d",&tempB);
    
    // 开始计算
    int pointerA = 0;
    int pointerB = 0;
    int ans = 0;
    for (int count = 0; count < (num_a + num_b + 1) / 2; count++) {
        if (pointerB >= num_b || (pointerA < num_a&&a[pointerA] < tempB)) {
            ans = a[pointerA];
            pointerA += 1;
        } else {
            ans = tempB;
            pointerB += 1;
            scanf("%d",&tempB);
        }
    }
    cout << ans << endl;
}
```

