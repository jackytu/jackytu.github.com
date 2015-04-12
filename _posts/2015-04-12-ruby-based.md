---
layout: post
title: "ruby programming based"
description: "ruby programming based"
category: ruby
tags: [ruby, based]
---
{% include JB/setup %}


## (1) 文件组织
### 1.1 文件结构
{% highlight ruby %}
>>>>tree -L 2
.
├── bin
│   ├── program
├── lib
│   ├── module1
│   │   ├── file1
│   │   ├── file2
├── test

{% endhighlight %}

### 1.2 代码组织
> 1. bin/program提供统一的对外接口(命令行解析等)
> 2. lib/module1/file{1,2}实现自包含，方便进行测试；


