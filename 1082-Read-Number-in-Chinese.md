---
title: <Python><PAT> 1082 Read Number in Chinese
date: 2019-09-02 11:18:59
tags:
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 读了那么久的中文，第一次觉得自己不懂中文。
---

# 题目

Given an integer with no more than 9 digits, you are supposed to read it in the traditional Chinese way. Output `Fu` first if it is negative. For example, -123456789 is read as `Fu yi Yi er Qian san Bai si Shi wu Wan liu Qian qi Bai ba Shi jiu`. Note: zero (`ling`) must be handled correctly according to the Chinese tradition. For example, 100800 is `yi Shi Wan ling ba Bai`.

### Input Specification:

Each input file contains one test case, which gives an integer with no more than 9 digits.

### Output Specification:

For each test case, print in a line the Chinese way of reading the number. The characters are separated by a space and there must be no extra space at the end of the line.

### Sample Input 1:

```in
-123456789
```

### Sample Output 1:

```out
Fu yi Yi er Qian san Bai si Shi wu Wan liu Qian qi Bai ba Shi jiu
```

### Sample Input 2:

```in
100800
```

### Sample Output 2:

```out
yi Shi Wan ling ba Bai
```

# 题解

## 思路

+ 这道题看起来不难，实际上牵涉到了0就很复杂
+ 我们先考虑不用0的，比如说“123456789”。
+ 如果将其反转的话，不难得出
+ 每隔4个数要输出“shi”，“bai”，“qian”
+ 具体点，是如果下标%4的结果是1,2,3，要分别加上shi，bai，qian
+ 对于万和亿来说，因为我们最多只有9位，所以可以当下标=4和=8的时候分别添加万和亿。万和亿不能取余，否则会对撞。
+ 所以我们可以这么做，将字符串反转
+ 遍历字符串
+ 当下标%4的结果是1,2,3，要分别加上shi，bai，qian
+ 当下标=4或8,分别加上wan和yi
+ 将字符都映射到汉字，添加进ans
+ 这样可以写出这样的代码（别忘了输出要逆序）

```python
digit = ["ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu", "shi"]
num, ans = input(), []
num = num[::-1]
for i, j in enumerate(num):
    if i % 4 == 1:
        ans.append("Shi")
    elif i % 4 == 2:
        ans.append("Bai")
    elif i % 4 == 3:
        ans.append("Qian")
    elif i == 4:
        ans.append("Wan")
    elif i == 8:
        ans.append("Yi")
    ans.append(digit[int(j)])
```

+ 这样子框架搭好了，可以处理0的问题了
+ 我们可以参照题目里`100800`这个例子。
+ 即 `yi Shi Wan ling ba Bai`
+ “ling”这个数什么时候要添加？
+ 我们看800的0，后面两个“ling”是都不读的。
+ 但是，十万**零**八百 这个0是要读的。
+ 所以可以概括为，要读的0，它的后面一个数一定不是0。（逆序过就是前面的数）
+ 所以条件可以写成`if j != "0" or (i >= 1 and num[i - 1] != "0")`
+ 但是这样还没完，我们这样的代码可以观察到，后面的800读成了八百十。
+ 也就是十百千也是要调整的。
+ 观察到，800的百要读，而后面的十不读，要读的原则其实就是这个数是不是0。不是0的，要读，是0的，不读
+ 但是万和亿都是必须要读的。
+ 最后别忘了几个特殊情况，数字完全等于0的，以及前面带负号的情况。这两个要在开头处理一下。
+ 理清这些还是有些复杂的，不过好在给的sample还不错，慢慢理能理好。

## 数据结构

+ num 是读来的字符串
+ ans是存放答案的列表

## 算法

+ 读数据
+ 为0直接输出0，结束
+ 如果有负号，先打印负号
+ 并且取数据除掉负号的部分
+ 将数字的字符串反转
+ 开始按照思路里的条件来遍历每一个字符
+ 输出即可

##  代码

+ 因为使用Python3能AC，因此只放了Python3的代码。

```python
digit = ["ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu", "shi"]
num, ans = input(), []
if num == "0":
    print("ling")
else:
    if num[0] == "-":
        print("Fu", end=" ")
        num = num[1:]
    num = num[::-1]
    for i, j in enumerate(num):
        if i % 4 == 1 and (num[i] != "0"):
            ans.append("Shi")
        elif i % 4 == 2 and (num[i] != "0"):
            ans.append("Bai")
        elif i % 4 == 3 and (num[i] != "0"):
            ans.append("Qian")
        elif i == 4:
            ans.append("Wan")
        elif i == 8:
            ans.append("Yi")
        if j == "-":
            ans.append("-")
        if j != "0" or (i >= 1 and num[i - 1] != "0"):
            ans.append(digit[int(j)])
    print(" ".join(list(map(str, ans[::-1]))))

```



