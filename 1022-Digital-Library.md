---
title: <Python><PAT> 1022 Digital Library
date: 2019-08-22 15:03:49
tags: 
- PAT
- 题解
- 哈希表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 充满挑战性的DFS
---

# 题目

A Digital Library contains millions of books, stored according to their titles, authors, key words of their abstracts, publishers, and published years. Each book is assigned an unique 7-digit number as its ID. Given any query from a reader, you are supposed to output the resulting books, sorted in increasing order of their ID's.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤104) which is the total number of books. Then *N* blocks follow, each contains the information of a book in 6 lines:

- Line #1: the 7-digit ID number;
- Line #2: the book title -- a string of no more than 80 characters;
- Line #3: the author -- a string of no more than 80 characters;
- Line #4: the key words -- each word is a string of no more than 10 characters without any white space, and the keywords are separated by exactly one space;
- Line #5: the publisher -- a string of no more than 80 characters;
- Line #6: the published year -- a 4-digit number which is in the range [1000, 3000].

It is assumed that each book belongs to one author only, and contains no more than 5 key words; there are no more than 1000 distinct key words in total; and there are no more than 1000 distinct publishers.

After the book information, there is a line containing a positive integer *M* (≤1000) which is the number of user's search queries. Then *M* lines follow, each in one of the formats shown below:

- 1: a book title
- 2: name of an author
- 3: a key word
- 4: name of a publisher
- 5: a 4-digit number representing the year

### Output Specification:

For each query, first print the original query in a line, then output the resulting book ID's in increasing order, each occupying a line. If no book is found, print `Not Found` instead.

### Sample Input:

```in
3
1111111
The Testing Book
Yue Chen
test code debug sort keywords
ZUCS Print
2011
3333333
Another Testing Book
Yue Chen
test code sort keywords
ZUCS Print2
2012
2222222
The Testing Book
CYLL
keywords debug book
ZUCS Print2
2011
6
1: The Testing Book
2: Yue Chen
3: keywords
4: ZUCS Print
5: 2011
3: blablabla
```

### Sample Output:

```out
1: The Testing Book
1111111
2222222
2: Yue Chen
1111111
3333333
3: keywords
1111111
2222222
3333333
4: ZUCS Print
1111111
5: 2011
1111111
2222222
3: blablabla
Not Found
```

# 题解

## 思路

+ 这道题虽然被判作30分的大题，但其实水得不行
+ 我们的任务是查询
+ 那么首先想到的就是哈希表
+ 我们不需要给书本单列一个数据结构，而是给书名，作者，关键字这些各一个哈希表
+ 其中，键是书名，作者，关键字等等，而值是包含多个ID的列表
+ 这样查询的时间就是O(1)了。
+ 这是一种以空间换时间的做法

## 数据结构

+ num_books 是书本的数量
+ hash_all是一个列表，保存了所有的哈希表，依次是书名，作者，关键字……等等。名字不重要，后面就直接用下标
+ num_que 是查询的次数

## 算法

+ 对每一个读到的书本，将它的各项信息添加到
+ 哈希表当中，键是信息，值是ID，要注意关键字，要分别给每一个关键字添加ID
+ 读取信息并输出

## 代码

使用Python3 能 AC，因此只放了Python的题解。

```python
from collections import defaultdict

num_books = int(input())
hash_all = [defaultdict(list), defaultdict(list), defaultdict(list), defaultdict(
    list), defaultdict(list)]

for _ in range(num_books):
    ID = input()
    for i, j in enumerate(hash_all):
        if i != 2:
            j[input()].append(ID)
        else:
            for key_word in input().split():
                j[key_word].append(ID)

num_que = int(input())
for _ in range(num_que):
    info = input()
    print(info)
    code, words = int(info[0]), info[3:]
    if len(hash_all[code - 1][words]) == 0:
        print("Not Found")
    for i in sorted(hash_all[code - 1][words]):
        print(i)

```

