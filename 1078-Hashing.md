---
title: <Python><PAT> 1078 Hashing
date: 2019-08-31 22:04:51
tags: 
- PAT
- 题解
- 哈希表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 考点是哈希表的二次平方探针法

---

# 题目

The task of this problem is simple: insert a sequence of distinct positive integers into a hash table, and output the positions of the input numbers. The hash function is defined to be $H(key) = key \%Tsize$ where $TSize$ is the maximum size of the hash table. Quadratic probing (with positive increments only) is used to solve the collisions.

Note that the table size is better to be prime. If the maximum size given by the user is not prime, you must re-define the table size to be the smallest prime number which is larger than the size given by the user.

### Input Specification:

Each input file contains one test case. For each case, the first line contains two positive numbers: $MSize(\leq 10 ^4)$ and $N(\leq MSize)$ which are the user-defined table size and the number of input numbers, respectively. Then *N* distinct positive integers are given in the next line. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the corresponding positions (index starts from 0) of the input numbers in one line. All the numbers in a line are separated by a space, and there must be no extra space at the end of the line. In case it is impossible to insert the number, print "-" instead.

### Sample Input:

```in
4 4
10 6 4 15
```

### Sample Output:

```out
0 1 4 -
```

# 题解

## 思路

+ 考你哈希表的二次探针法还记不记得
+ 要是忘了就凉透了
+ 总共就两步
+ 先找下一个素数
+ 再使用二次探针法找空位

## 数据结构

+ size 哈希表的尺寸
+ step 二次探针法的步长
+ ans 保存答案
+ hashed 是一个列表，用以记录这个位置是不是空着的
  + 下标就是对应哈希表的位置
  + 值是True或False，代表被填了还是没被填

## 算法

+ 找到下一个质数
  + 如果是1或者2，直接返回2
  + 如果是偶数，size先加一
  + 遍历从3到sqrt(size)的所有奇数
  + 如果能被size能被这些奇数整除，说明size不是素数
  + size加2,重新遍历从3到sqrt(size)的所有奇数
  + 直到找到一个size，它不能被从3到sqrt(size)的所有奇数整除
  + 这个算法相对复杂度低一点
+ 二次探针法
  + 先建立一个`hashed[size]`。
  + 遍历step从0到size-1
  + 每次使key = (num % size + step ** 2) % size。
  + 当hashed对应key的位置是空的时候
  + 标记这个位置并填满，同时答案添加这个key
  + 当遍历完了还没有找到空位时
  + 答案添加‘-’
+ 输出答案

## 代码

+ 由于使用Python能AC，因此只放了Python的解。

```python
size, num_input = list(map(int, input().split()))
# 找到size的下一个质数
if size == 1 or size == 2:
    size = 2
else:
    if size % 2 == 0:
        size += 1
    while True:
        for i in range(3, int(size ** 0.5) + 1, 2):
            if size % i == 0:
                size += 2
                break
        else:
            break
# 开始填充
hashed = [False for _ in range(size)]
ans = []
nums = list(map(int, input().split()))
for num in nums:
    for step in range(0, size):
        key = (num % size + step ** 2) % size
        if not hashed[key]:
            ans.append(key)
            hashed[key] = True
            break
    else:
        ans.append("-")
print(" ".join(list(map(str, ans))))

```

