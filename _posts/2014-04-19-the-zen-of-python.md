---
layout: post
title: "the zen of python"
description: "the zen of python"
category: python
tags: [python]
---
{% include JB/setup %}

```python
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
'追求优美的代码'

Explicit is better than implicit.
'优美的代码需要简洁明了，不要避免晦涩难懂的代码逻辑'

Simple is better than complex.
'追求简单：优美的代码避免不必要的复杂度，如类的属性不要超过7个'

Complex is better than complicated.
'如果复杂不可避免，也不能混乱'

Flat is better than nested.
'优美的代码应该是扁平的，不要有嵌套的网状结构'

Sparse is better than dense.
'优美的代码应该尽量稀疏，不要希望一行代码解决所有问题(与ruby相反), ruby习惯do_something if condition, 而python则不建议将逻辑放在一行之内'

Readability counts.
'优美的代码应该有足够好的可读性'

Special cases aren't special enough to break the rules.
Although practicality beats purity.
'即使希望实用性，也不要违背可读性的规则'

Errors should never pass silently.
Unless explicitly silenced.
'优美的代码应该精准地捕捉与处理错误，比如，在捕捉异常不能只捕捉Exception，而要细化，除非在一些特殊情况下只能pass'

In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
’当存在多种可能性时，不要尝试去猜测，而是找一种，而且仅有一种最好的方案来解决，比如，用map+lambda，还不如用推导式，其实这并不容易，因为你不是python之父'

Now is better than never.
Although never is often better than *right* now.
'做好过不做，但不假思索地动手还不如不动手'

If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
’如果你的方法很难描述，那肯定不是一个好方案，反之，如果很容易解释，则可能是一个好方法‘

Namespaces are one honking great idea -- let's do more of those!
'名字空间是非常好的方案，我们多加应用'
```

