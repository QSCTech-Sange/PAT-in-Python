---
title: <Python><PAT> 1071 Speech Patterns
date: 2019-08-30 13:54:23
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 看到字符串，请使用——
---

# 题目

People often have a preference among synonyms of the same word. For example, some may prefer "the police", while others may prefer "the cops". Analyzing such patterns can help to narrow down a speaker's identity, which is useful when validating, for example, whether it's still the same person behind an online avatar.

Now given a paragraph of text sampled from someone's speech, can you find the person's most commonly used word?

### Input Specification:

Each input file contains one test case. For each case, there is one line of text no more than 1048576 characters in length, terminated by a carriage return `\n`. The input contains at least one alphanumerical character, i.e., one character from the set [`0-9 A-Z a-z`].

### Output Specification:

For each test case, print in one line the most commonly occurring word in the input text, followed by a space and the number of times it has occurred in the input. If there are more than one such words, print the lexicographically smallest one. The word should be printed in all lower case. Here a "word" is defined as a continuous sequence of alphanumerical characters separated by non-alphanumerical characters or the line beginning/end.

Note that words are case **insensitive**.

### Sample Input:

```in
Can1: "Can a can can a can?  It can!"
```

### Sample Output:

```out
can 5
```

# 题解

## 思路

+ 题目里要求我们找出现最频繁的单词和数量
+ 毫无疑问用Python的Counter类是最为便捷的
+ 要注意几点
+ 首先，题目是大小写不分的，这个好办，统一`.lower()`改成小写，也便于输出
+ 其次，单词是由数字和字母组成的，那么我们可以用`.isalnum()`来判断
+ 我们可以将原字符串中的特殊符号都替换成空格，当作分割符
+ 注Python的字符串是不可变对象，所以要先转成列表
+ 然后将特殊字符改为空格
+ 然后再`“”.join()`连回一个字符串
+ 使用`.split()`将它拆分成单词，并使用Counter计算每个单词的出现次数
+ 最后利用counter的`.most_common(1)`输出即可
+ 且慢！这样忽略了一点，就是最多重复的不一定只有一个单词，题目让我们输出所有这些单词，按照字母序。而most_common(1)只会输出一个。
+ 那我们可以先记录最多重复的次数
+ 然后将counter的键按照字母序排列
+ 遍历这些键，如果等于最多重复次数的，输出即可。
+ 当然也可以先找到最大的那几个，再进行排序。
+ 两种方法都cover到了，但是主体前面的是一样的。

## 数据结构

+ line 是原来字符串
+ count 计数器
+ max 最多重复次数
+ 第一种方法里
  + keys 是全部单词
+ 第二种方法里
  + keys 是最多出现的那些单词

## 算法

+ 对line清理掉无关字符
+ 使用计数器统计单词重复频次
+ 记录最多出现的次数
+ 两种方法
  + 一种是，先排列所有单词，然后遍历，等于最多出现次数的那些单词被输出。
  + 另一种是，先找到等于出现次数的那些单词，然后排序并输出。

## 代码

+ 由于使用Python能AC，因此只写了Python的题解。
+ 第二种方法更快一点。第一种有时候会超时。

### Python 先排序，再找最大

```python
from collections import Counter

line = [i for i in input()]
for i, j in enumerate(line):
    if not j.isalnum():
        line[i] = " "
line = "".join(line)
count = Counter(line.lower().split())
max = count.most_common(1)[0][1]
keys = sorted(count)
for i in keys:
    if count[i] == max:
        print(i, count[i])

```

### Python 先找最大，再排序

```python
from collections import Counter

line = [i for i in input()]
for i, j in enumerate(line):
    if not j.isalnum():
        line[i] = " "
line = "".join(line)
count = Counter(line.lower().split()).most_common()
max = count[0][1]
keys = []
for i, j in count:
    if j == max:
        keys.append(i)
    else:
        break
keys.sort()
for i in keys:
    print(i, max)
```

