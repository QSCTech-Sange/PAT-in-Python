---
title: <Python><PAT> 1057 Stack
date: 2019-08-28 13:37:51
tags: 
- PAT
- 题解
- 桶排
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 高难度，不用树状数组，红黑树等方法的话很有挑战性，而前面几个又是超纲内容。
---

# 题目

Stack is one of the most fundamental data structures, which is based on the principle of Last In First Out (LIFO). The basic operations include Push (inserting an element onto the top position) and Pop (deleting the top element). Now you are supposed to implement a stack with an extra operation: PeekMedian -- return the median value of all the elements in the stack. With *N* elements, the median value is defined to be the (*N*/2)-th smallest element if *N* is even, or $((N+1) / 2)$-th if *N* is odd.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer $N\left( \leq 10^{5}\right)$. Then *N* lines follow, each contains a command in one of the following 3 formats:

```
Push key
Pop
PeekMedian
```

where `key` is a positive integer no more than 105.

### Output Specification:

For each `Push` command, insert `key` into the stack and output nothing. For each `Pop` or `PeekMedian` command, print in a line the corresponding returned value. If the command is invalid, print `Invalid` instead.

### Sample Input:

```in
17
Pop
PeekMedian
Push 3
PeekMedian
Push 2
PeekMedian
Push 1
PeekMedian
Pop
Pop
Push 5
Push 4
PeekMedian
Pop
Pop
Pop
Pop
```

### Sample Output:

```out
Invalid
Invalid
3
2
2
1
2
4
4
5
3
Invalid
```

# 题解

## 思路

+ 这道题首先可以想到的是暴力法
+ 即模拟一个栈，每次输出的时候排序找中值
+ 会过两个点
+ 另一种思想是使用桶排
+ 首先模拟一个栈
+ 但是要增加一个长度为100000的数组
+ 称为10万个桶，记录一个数出现了多少次。下标是数，值是这个数出现了多少次。
+ 如果用数插进来了，更新栈的同时
+ 桶的对应下标也要加一
+ pop的同时也更新桶
+ 在查找的时候，先计算要找从小到大的第几个数
+ 再从桶的下标0开始遍历
  + 不断记录count，它不断加上桶对应下标的值
  + 当count到了要找的数的时候，输出它并break
+ 然而这种方法也会超时
+ 因为十万个桶遍历起来依然太慢了。
+ 更好地方法是设立双层桶。
+ 第一个桶存放每1000个数出现了几次
+ 第二个桶是双层的，第一层有100个，对应于第1个桶的下标，第二层对应具体的数。
+ 比如说我读了一个数，叫32145，那么给第一个桶的下标32的值加一
+ 然后给第二个桶的第一层的第32个，对应这个第32个桶的第145个下标加一
+ 这样遍历次数最多就是1000+100次，从十万次减少到了1100次
+ 这样做的弊端就是空间复杂度会增加，又需要十万个int类型的空间。
+ C++使用双层桶已经可以过全部测试点了
+ 可以想到，可以玩三层桶，四层桶的方向上做。
+ 然而在Python中，即便使用三层桶，将遍历次数减少到210次，也会超时。三层桶已经比较抽象了。
+ 还有诸如树状数组，红黑树之类的超纲算法就不讨论了。

## 数据结构

+ stk 是真实的栈
+ bucket 是桶

## 算法

+ 算法基本都写在题解里了

+ 双层三层桶理解得困难的话，可以先从单层桶理解。

## 代码

+ 这道题对时间咬得很近，经测试C++在双层的情况下能通过，而Python即便在三层的状态下也通过不了。

### 暴力法

+ 每次输出中值都排序一下，这个方法只能过两个点。

```python
stk = []    # 栈
commands = int(input())
for _ in range(commands):
    info = input()
    if info == "Pop":
        if len(stk) == 0:
            print("Invalid")
        else:
            print(stk.pop())
    elif info == "PeekMedian":
        if len(stk) != 0:
            print(sorted(stk)[(len(stk) - 1)//2])
        else:
            print("Invalid")
    else:
        num = int(info.split()[1])
        stk.append(num)

```

### 桶排法

+ 只过了两个点，耗时还更多了……

```python
bucket = [0 for _ in range(100001)]
stk = []
commands = int(input())
for _ in range(commands):
    info = input()
    if info == "Pop":
        if len(stk) == 0:
            print("Invalid")
        else:
            bucket[stk[-1]] -= 1
            print(stk.pop())
    elif info == "PeekMedian":
        if len(stk) != 0:
            target = (len(stk) + 1) // 2
            index = 0
            for num, count in enumerate(bucket):
                index += count
                if index >= target:
                    print(num)
                    break
        else:
            print("Invalid")
    else:
        num = int(info.split()[1])
        bucket[num] += 1
        stk.append(num)

```

### 二层桶排法 Python

+ 由于对Python不友好，依然只能过两个点

```python
bucket_count = [0 for _ in range(100)]
bucket = [[0 for _ in range(1000)] for _ in range(100)]
stk = []
commands = int(input())
for _ in range(commands):
    info = input()
    if info == "Pop":
        if len(stk) == 0:
            print("Invalid")
        else:
            num = stk[-1]
            bucket[num // 1000][num % 1000] -= 1
            bucket_count[num // 1000] -= 1
            print(stk.pop())
    elif info == "PeekMedian":
        if len(stk) != 0:
            target = (len(stk) + 1) // 2
            index = 0
            for num, count in enumerate(bucket_count):
                index += count
                if index >= target:
                    index -= count
                    for num_2, count_2 in enumerate(bucket[num]):
                        index += count_2
                        if index >= target:
                            print(num * 100 + num_2)
                            break
                    break
        else:
            print("Invalid")
    else:
        num = int(info.split()[1])
        bucket_count[num // 1000] += 1
        bucket[num // 1000][num % 1000] += 1
        stk.append(num)

```



### 二层桶排法 C++

+ 稳

```c++
#include <algorithm>
#include <iostream>
#include <vector>
#include <string>

using namespace std;


int main() {
    int bucket_count[100];
    for (int &i : bucket_count)
        i = 0;
    int bucket[100][1000];
    for (auto &i : bucket) {
        for (int &j : i)
            j = 0;
    }
    vector<int> stk;
    int commands;
    scanf("%d", &commands);
    for (int i = 0; i < commands; i++) {
        string command;
        command.resize(10);
        cin >> command;
        if (command == "Pop") {
            if (stk.size() == 0)
                printf("Invalid\n");
            else {
                int num = stk.back();
                bucket[num / 1000][num % 1000] -= 1;
                bucket_count[num / 1000] -= 1;
                printf("%d\n", stk.back());
                stk.pop_back();
            }
        } else if (command == "PeekMedian") {
            if (!stk.empty()) {
                int target = (stk.size() + 1) / 2;
                int index = 0;
                for (int num = 0; num < 100; num++) {
                    index += bucket_count[num];
                    if (index >= target) {
                        index -= bucket_count[num];
                        for (int num_2 = 0; num_2 < 1000; num_2++) {
                            index += bucket[num][num_2];
                            if (index >= target) {
                                printf("%d\n", num * 1000 + num_2);
                                break;
                            }
                        }
                        break;
                    }

                }
            } else
                printf("Invalid\n");
        } else {
            int num;
            scanf("%d", &num);
            bucket_count[num / 1000] += 1;
            bucket[num / 1000][num % 1000] += 1;
            stk.push_back(num);
        }
    }
}
```

### 三层桶排法 Python 

+ Python用三层依然过不了，只有两个点能过。

```python
bucket_1 = [0 for _ in range(100)]
bucket_2 = [[0 for _ in range(100)] for _ in range(100)]
bucket_3 = [[[0 for _ in range(10)] for _ in range(100)] for _ in range(100)]
stk = []
commands = int(input())
for _ in range(commands):
    info = input()
    if info == "Pop":
        if len(stk) == 0:
            print("Invalid")
        else:
            num = stk[-1]
            bucket_3[num // 1000][(num % 1000)//10][num % 10] -= 1
            bucket_2[num // 1000][(num % 1000)//10] -= 1
            bucket_1[num // 1000] -= 1
            print(stk.pop())
    elif info == "PeekMedian":
        if len(stk) != 0:
            target = (len(stk) + 1) // 2
            index = 0
            for num, count in enumerate(bucket_1):
                index += count
                if index >= target:
                    index -= count
                    for num_2, count_2 in enumerate(bucket_2[num]):
                        index += count_2
                        if index >= target:
                            index -= count_2
                            for num_3, count_3 in enumerate(bucket_3[num][num_2]):
                                index += count_3
                                if index >= target:
                                    print(num * 1000 + num_2 * 10 + num_3)
                                    break
                            break
                    break
        else:
            print("Invalid")
    else:
        num = int(info.split()[1])
        bucket_3[num // 1000][(num % 1000)//10][num % 10] += 1
        bucket_2[num // 1000][(num % 1000)//10] += 1
        bucket_1[num // 1000] += 1
        stk.append(num)

```

