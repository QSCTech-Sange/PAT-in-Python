---
title: <Python><PAT> 1044 Shopping in Mars
date: 2019-08-26 14:25:18
tags: 
- PAT
- 题解
- 二分查找
- 双指针
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 暴力虽好，时间过不了。这题的二分查找，有点巧。
---

# 题目

Shopping in Mars is quite a different experience. The Mars people pay by chained diamonds. Each diamond has a value (in Mars dollars M\$). When making the payment, the chain can be cut at any position for only once and some of the diamonds are taken off the chain one by one. Once a diamond is off the chain, it cannot be taken back. For example, if we have a chain of 8 diamonds with values M\$3, 2, 1, 5, 4, 6, 8, 7, and we must pay M​\$15. We may have 3 options:

1. Cut the chain between 4 and 6, and take off the diamonds from the position 1 to 5 (with values 3+2+1+5+4=15).
2. Cut before 5 or after 6, and take off the diamonds from the position 4 to 6 (with values 5+4+6=15).
3. Cut before 8, and take off the diamonds from the position 7 to 8 (with values 8+7=15).

Now given the chain of diamond values and the amount that a customer has to pay, you are supposed to list all the paying options for the customer.

If it is impossible to pay the exact amount, you must suggest solutions with minimum lost.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 numbers: $N\left( \leq 10^{5}\right)$, the total number of diamonds on the chain, and $M\left( \leq 10^{8}\right)$, the amount that the customer has to pay. Then the next line contains *N* positive numbers $D_{1} \cdots D_{N}\left(D_{i} \leq 10^{3} \text { for all } i=1, \cdots, N\right)$ which are the values of the diamonds. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print `i-j` in a line for each pair of `i` ≤ `j` such that *D*`i` + ... + *D*`j` = *M*. Note that if there are more than one solution, all the solutions must be printed in increasing order of `i`.

If there is no solution, output `i-j` for pairs of `i` ≤ `j` such that *D*`i` + ... + *D*`j` >*M* with (*D*`i` + ... + *D*`j` −*M*) minimized. Again all the solutions must be printed in increasing order of `i`.

It is guaranteed that the total value of diamonds is sufficient to pay the given amount.

### Sample Input 1:

```in
16 15
3 2 1 5 4 6 8 7 16 10 15 11 9 12 14 13
```

### Sample Output 1:

```out
1-5
4-6
7-8
11-11
```

### Sample Input 2:

```in
5 13
2 4 5 7 9
```

### Sample Output 2:

```out
2-4
4-5
```

# 题解

## 思路

+ 首先容易想到暴力解

+ 即遍历每一个数

  + 先设sum为0
  + 从这个数再遍历
    + sum不断加上下一个数，使sum刚好大于或等于要付的钱
  + 判断sum和之前的min_cost来确定是否要更新答案

+ 输出

+ 容易写出这样的代码

+ ```python
  num_diamonds, pay = list(map(int, input().split()))	
  chain = list(map(int, input().split()))
  ans = []
  min_cost = 99999
  for i in range(num_diamonds):
      sum = 0
      for j in range(i, num_diamonds):
          sum += chain[j]
          # 判断要不要更新ans
          if pay <= sum:
              if sum < min_cost:
                  ans = [[i, j]]
                  min_cost = sum
              elif sum == min_cost:
                  ans.append([i, j])
                  min_cost = sum
              break
  for i in ans:
      print("-".join(list(map(lambda x: str(x + 1), i))))
  
  ```

+ 但是这样会有大量的重复计算，造成严重的超时。

+ 更好的方式是二分查找

+ 首先把从第一个数开始求和的结果放在一个列表里

+ 即`[a[0], a[0]+a[1], a[0]+a[1]+a[2],...]`

+ 为了方便起见，前面再加一个0,即`0, a[0], a[0]+a[1], a[0]+a[1]+a[2],...]` 这个0待会要用到

+ 此时这个新建的列表，我们称为`_sum` ，是一个有序的，从小到大排列的。

+ 如果我们找，从第一个数字开始，加到正好能使总和大于等于要付的钱的第一个数

+ 就相当于在`_sum`找第一个大于等于要付的钱的数

+ 这就可以使用二分查找了

+ 可是从第二个数字开始呢？难道要再算走一遍`_sum`吗？

+ 不必。

+ 对于第二个数字开始，它的`_sum` 其实就是原来的`_sum` 减去`_sum[1]`。因为`_sum[1]`表示的就是第一个数。

+ 同理，对于第N个数，它的`_sum`和列表就是最初的`_sum`剪去`_sum[N-1]`。因为从N到K的和就是从1到K的和减去从1到N的和。

+ 那么对每个数走一遍二分查找即可。

+ 因为我们是沿着下标顺序找的，所以最后不用排序。

+ 还有一种方法是**双指针法**，没有二分查找帅，就不用了。

## 数据结构

+ _sum 记录从0开始的累加和
+ l 是二分查找左边界
+ r 是二分查找右边界
+ mid 是二分查找的中间
+ num临时变量，记录对i这个数来说，比要付钱略微大的数
+ ans 是一个列表，记录要输出的答案
+ ans的一项代表一组答案，是一个列表，包含两个数，即下标。

## 算法

+ 都写在思路里了。

## 代码

+ 因为此题对Python歧视，不管暴力解还是二分查找都有三个点超时。建议使用C++。原理都是一样的。

### Python

```python
num_diamonds, pay = list(map(int, input().split()))
chain = list(map(int, input().split()))
_sum = [0] + [sum(chain[:i + 1]) for i in range(num_diamonds)]
ans = []
min_cost = _sum[-1]
for i in range(num_diamonds):	# 二分查找
    l = i
    r = num_diamonds - 1
    while l < r:
        mid = (l + r) // 2
        if _sum[mid + 1] - _sum[i] < pay:
            l = mid + 1
        else:
            r = mid
    num = _sum[l + 1] - _sum[i] # 看情况更新答案
    if num >= pay:
        if num < min_cost:
            ans = [[i + 1, l + 1]]
            min_cost = num
        elif num == min_cost:
            ans.append([i + 1, l + 1])
            min_cost = num

for i in ans:
    print("-".join(list(map(str, i))))

```

### C++

```c++
#include <iostream>
#include <vector>
using namespace std;

int main(){
    int num_diamonds,pay;
    scanf("%d %d",&num_diamonds,&pay);
    vector<int>sum(num_diamonds+1,0);
    for(int i = 1;i<=num_diamonds;i++){
        scanf("%d",&sum[i]);
        sum[i] += sum[i-1];
    }
    vector<vector<int>> ans;
    int min_cost = sum[num_diamonds];
    for (int i = 0;i < num_diamonds;i++){ // 二分查找
        int l = i;
        int r = num_diamonds - 1;
        while (l < r){
            int mid = (l + r) / 2;
            if (sum[mid + 1] - sum[i] < pay)
                l = mid + 1;
            else r = mid;
        }
        int num = sum[l+1] - sum[i];    // 视情况更新答案
        if (num >= pay){
            if (num < min_cost){
                ans = {vector<int>{i+1,l+1})};
                min_cost = num;
            }else if (num == min_cost){
                ans.emplace_back(vector<int>{i+1,l+1});
                min_cost = num;
            }
        }
    }
    for (auto &it:ans)
        printf("%d-%d\n",it[0],it[1]);
}
```

