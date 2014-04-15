---
layout: post
title: "python comprehension"
description: ""
category: python
tags: [python]
---
{% include JB/setup %}

## 数组转换
先考虑一类最基础的场景：给定一个数组，每个元素是小写字母，现在需要将每个元素转换为大写字母，你会如何处理？  
你可以这样处理：  
```python
>>> input = ['a', 'b', 'c']
>>> output = []
>>> for s in input:
...     output.append(s.upper())
...
>>> print output
['A', 'B', 'C']
```


看上去没有什么问题，但如果你了解推导式，你可能会考虑的解决方案：    


```python
>>> input = ['a', 'b', 'c']
>>> output = [s.upper() for s in input]
>>> print output
['A', 'B', 'C']
```


看到区别了吗，初始化不见了，append()也不见了，这就是简洁之美；

##列表推导式
  
Python 列表推导式，是一种用于基于list的计算语法，简洁地将一个list中的每个元素按照需要的逻辑转换成另外一个数组；


一个列表推导式，包括几个基本要素：


* 列表：推导式的输入与输出均是一个list；
* 逻辑：逻辑简言之就是一个转换函数，也可以是一个lambda表达式；


推导式(comprehension)其实等价于map + lambda的功能，比如，针对写程序时经常会需要生成随机串，基于map+lambda的
demo如下：


```python
>>> import os
>>> inbox = ''.join(map(lambda xx: hex(ord(xx))[2:], os.urandom(16)))
>>> print inbox
ec8259aaca60e899b7dd1c9e1f74a2e
```


通过推导式实现就是：


```python
>>> import os
>>> inbox = ''.join([hex(ord(i))[2:] for i in os.urandom(16)])
>>> print inbox
30d8758580b546c451db8a5c4ebf66b2
```

两种方式其实简洁程度上区别不大，但pep的风格建议中，更偏向于推导式，这样会减少函数的调用，效率也会更高；

