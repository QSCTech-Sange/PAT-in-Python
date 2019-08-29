---
title: <Python><PAT> 1067 Sort with Swap(0, i)
date: 2019-08-29 17:31:34
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 这道题输入格式并不是按照题目说的来的，同时使用Python的话有交换的坑，而且会有两个点超时，综合这三点，这道题不建议使用Python做。
---

# 题目

Given any permutation of the numbers {0, 1, 2,..., *N*−1}, it is easy to sort them in increasing order. But what if `Swap(0, *)` is the ONLY operation that is allowed to use? For example, to sort {4, 0, 2, 1, 3} we may apply the swap operations in the following way:

```
Swap(0, 1) => {4, 1, 2, 0, 3}
Swap(0, 3) => {4, 1, 2, 3, 0}
Swap(0, 4) => {0, 1, 2, 3, 4}
```

Now you are asked to find the minimum number of swaps need to sort the given permutation of the first *N* nonnegative integers.

### Input Specification:

Each input file contains one test case, which gives a positive $N(\leq 10 ^ 5)$ followed by a permutation sequence of {0, 1, ..., *N*−1}. All the numbers in a line are separated by a space.

### Output Specification:

For each case, simply print in a line the minimum number of swaps need to sort the given permutation.

### Sample Input:

```in
10
3 5 7 2 6 4 9 0 8 1
```

### Sample Output:

```out
9
```

# 题解

## 思路

+ 不建议使用Python做，原因写在代码里了。
+ 这道题属实有点抽象。
+ 对于一个数字从0到N-1的乱序序列，我们只能交换0和别的数
+ 问怎么样才能用最少的移动步数把它们排列整齐？
+ 能想出移动方法简单，但是难以判断是不是步数最少的。
+ 实际上你想的每种方法都是步数最少的（除非你反复交换相同的数故意浪费步数）
+ 至于怎么证，我也不知道。
+ 我猜想你想的方法一定是这样
+ 每次移动0,都尽量让一个数一步到达它的真实位置，即一个贪心的思想，如果0在第2位，那就先把2和它交换。
+ 比如说，`2 1 0 3` 这四个数，第2位（从0开始数）应该是2但是是0,你肯定会想着先把0和2交换，这样2就在了正确的位置
+ 我们把这个思路形式化。
+ 首先找到0在的位置`i`
+ 如果0不在第0位，即`i!=0`
  + 这说明0的位置应该摆放的数是`i`，而实际上摆放了0
  + 那这说明`i`也错位了。
  + 我们去找到`i`的所在位置，假定在`j`
  + 那么我们的0（位置`i`） 去和`i`（位置在`j`）交换
  + 这样`i`就被放到了位置`i`，找到了正确的位置，而`j`移动到了
  + 重复此步骤，直到0被放到了首位。
+ 这样可以使得一部分数排列整齐了，准确地说是从0开始找位置下标直到找到0形成的闭环。
+ 那么我们就得从头（这次是1）开始遍历，每找到一个`i`不在第`i`位的，需要开始交换。假设`i`在位置`j`。
+ 这次交换的时候，`0`已经在第`0`位了，我们将`0`与`i`交换（即`0`到了位置`j`，而`i`到了位置`0`）
+ 这时候`0`不在位置`0`了而在位置`j`了，我们就可以先找到数`j`，然后和它交换（也就是重复上面如果0不再0位的那一串循环）
+ 每次交换别忘了增加count
+ 输出即可
+ 为了方便找到索引，我们开了一个index数组，存放的下标是数，而值是这个数被摆放到的位置。我们交换的时候，只要改index数组就可以了。用index数组找位置就会方便很多，例如`index[1]= 0`表示数字1的位置是0

## 数据结构

+ index 是一个数组
  + 下标是数
  + 值是这个数目前被排在了哪里

## 算法

+ 参照思路

## 代码

+ 不建议使用Python，请使用C++

### Python3

+ 调整为了测试点真正的数据输入格式（即所有数据都是在一行，并没有跨行）。
+ 讲一下为什么后面` index[0], index[i] = index[i], index[0]`可以使用Python独有的交换，而前面不能写成`index[index[0]], index[0] = index[0],index[index[0]]`
+ 因为`index[0]`是会改变的，所以不能这么写。这点非常得坑。
+ 有两个点会超时。

```python
numbers = list(map(int, input().split()[1:]))
index = [numbers.index(i) for i in range(len(numbers))]
count = 0
for i in range(1, len(numbers)):
    if index[i] != i:
        while index[0] != 0:
            temp = index[index[0]]
            index[index[0]] = index[0]
            index[0] = temp
            count += 1
        if index[i] != i:
            index[0], index[i] = index[i], index[0]
            count += 1
print(count)

```

### C++

+ 稳。

```c++
#include <iostream>
using namespace std;
int main() {
    int num, temp, count = 0;
    scanf("%d",&num);
    int index[num];
    for(int i = 0; i < num; i++){
        scanf("%d",&temp);
        index[temp] = i;
    }
    for(int i = 1; i < num; i++) {
        if(i != index[i]) {
            while(index[0] != 0) {
                swap(index[0],index[index[0]]);
                count++;
            }
            if(i != index[i]) {
                swap(index[0],index[i]);
                count++;
            }
        }
    }
    printf("%d\n",count);
    return 0;
}
```

