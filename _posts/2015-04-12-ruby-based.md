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
>>>> ruby -I lib bin/anagram teaching code
{% endhighlight %}

### 1.2 代码组织
> 1. bin/program提供统一的对外接口(命令行解析等)
> 2. lib/module1/file{1,2}实现自包含，方便进行测试；

## (2) 关于编码
> 在大型软件中，最好指定默认的文件编码,utf-8是通用的编码，在国外的浏览器显示中文也不用下载专门的字符集；
> ruby1.9默认采用us-ascii编码，如果没有指定编码，使用了byte value超过127的字符，将会报错
> ruby2.0默认采用utf-8编码，下面例子中的π的size则为1;
> 字符间的编码可以通过encoding方法，但如果目标字符集中没有需要转换的字符，将会报错；
{% highlight ruby %}
>>>cat ascii.rb
π = 3.14159
puts "π = #{π}"
>>>ruby test.rb
invalid multibyte char (US-ASCII)

>>> cat utf8.rb
# encoding = utf-8
π = 3.14159
puts "π = #{π}"
>>> ruby utf8.rb
π = 3.14159
{% endhighlight %}
> 在处理文件时，由于本身运行环境中有外部的编码，可以在读入时指定编码，避免程序无法处理；
{% highlight ruby %}
f = File.open("/etc/passwd", "r:ascii")
puts "File encoding is #{f.external_encoding}"
line = f.gets
puts "Data encoding is #{line.encoding}"
{% endhighlight %}
> 有时候可能存在外部字符无法映射到正确的字符，可以使用map的方式进行映射；
{% highlight ruby %}
f = File.open("iso-8859-1.txt", "r:iso-8859-1:utf-8")
puts f.external_encoding.name
line = f.gets
puts line.encoding
puts line
{% endhighlight %}
> Ruby支持-E参数来指定外部编码(写入文件/读入文件)与内部编码(读入并处理文件);
{% highlight ruby %}
ruby -E iso-8859-1:utf-8
{% endhighlight %}

## (4) 交互式ruby
> irb支持tab键联想(irb -r)；
> irb支持subsession，通过load的方式载入文件，可以重复加载，而require不行；
{% highlight ruby %}
irb
irb(main):001:0> self
=> main
irb(main):002:0> irb "whoami"
irb#1(whoami):001:0> self
=> "whoami"
irb#1(whoami):002:0> irb_exit
=> #<IRB::Irb: @context=#<IRB::Context:0x007fb95c9f5138>, @signal_status=:IN_EVAL, @scanner=#<RubyLex:0x007fb95c9ee1d0>>
irb(main):003:0> self
=> main
irb(main):004:0>
{% endhighlight %}
> irb可以通过.irbrc文件指定配置文件，配置提示符之类，也可以使用命令,help，irb_exit之类；
> ruby的web，自带cgi模块，与模板语言erb,haml等









