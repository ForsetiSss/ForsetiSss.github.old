---
title: python中的lambda和map函数
author: Shi Daming
date: 2021-05-14 23:00:00 +0800
categories: jekyll update
tags: [python, lambda, map]
pin: true
---

**目录**

\- [1、lambda函数](#1lambda函数)

\- [2、map函数](#2map函数)

\- [3、map函数和lambda函数结合](#3map函数和lambda函数结合)


### 1、lambda函数

他就是个没有名字的函数，比如匿名信这种。我们能在某一函数参数条件中直接调用他、或者借用其返回的实体构成新的函数名 如下。

比如说  lambda x,y: x+y  就是说我的函数输入x,y，返回x+y

```python
y=lambda a,b,c:a+b+c
print('\n',y(1,2,3))
```


结果为6

仔细看看，是不是y成为了新的函数名？！

 

### 2、map函数

map就是映射的意思，他肯定是将两种东西结合映射为某一个结果。他就是接收一个函数function和一个list列表，并通过把函数f依次作用在list的每一个元素，从而得到一个新的list返回（py3中返回一个map对象，用list函数转换一下即可）

```python
def fib_recur(n):
    if n<=1:
        return n
    else:
        return fib_recur(n-1)+fib_recur(n-2)


X=input().strip().split()
N=list(map(int, X))[0]

for i in range(1, N):
    print(fib_recur(i),end=' ')
```


注意哈这个X必须是列表，int则表示函数了，当然也可以其他函数命名

```python
def fib_recur(n):
    if n<=1:
        return n
    else:
        return fib_recur(n-1)+fib_recur(n-2)


def ex(c):
    return int(c)
        
X=input().strip().split()
N=list(map(ex, X))[0]

for i in range(1, N):
    print(fib_recur(i),end=' ')
```


注意他是自动迭代地对X列表的每一个元素操作，也就是说map自带迭代器！

 

### 3、map函数和lambda函数结合

显然只用改变map函数中的函数体就可以，无非就是函数体没有了实名，将lambda放于map的参数条件之中

```python
def fib_recur(n):
    if n<=1:
        return n
    else:
        return fib_recur(n-1)+fib_recur(n-2)

def ex(c):
    return int(c)
        
##X=input().strip().split()
##N=list(map(ex, X))[0]
##
##for i in range(1, N):
##	print(fib_recur(i),end=' ')

X=input().strip().split()
N=list(map(lambda a:int(a), X))[0]

for i in range(1, N):
    print(fib_recur(i),end=' ')
```

在本代码list(map(lambda a:int(a), X)) 这一句只用注意两个点：

1） lambda参数只能是一个参数标量，因为map是自动对列表X的元素迭代的

2）X为一维情况，所以他的每一个元素是标量

针对X为二维情况，博主暂时没想到好的解决办法，只能是X[2]来选择其中的一个向量列表。

```python
def fib_recur(n):
    if n<=1:
        return n
    else:
        return fib_recur(n-1)+fib_recur(n-2)


X=[[1,2,3],[2,4,6],[3,6,9]]
N=list(map(lambda a:int(a), X[2]))[2]

for i in range(1, N):
    print(fib_recur(i),end=' ')
```

————————————————
版权声明：本文为CSDN博主「lamusique」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/lamusique/article/details/89162363