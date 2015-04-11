---
layout: post
title: "ruby tricks"
description: "ruby programming advanced - tricks"
category: ruby
tags: [ruby, tricks]
---
{% include JB/setup %}


## (1) Trace技巧
>TRICK1: ruby2.0可以使用TracePoint来进行调用trace;
>TRICK2: ruby1.9则可以使用caller方法
{% highlight ruby %}
def func_1
  puts "1"
end
TracePoint.trace do |tp|
  p tp
end

func_1
>>>>
#<TracePoint:c_return `trace'@trace.rb:4>
#<TracePoint:line@trace.rb:8>
#<TracePoint:call `func_1'@trace.rb:1>
#<TracePoint:line@trace.rb:2 in `func_1'>
#<TracePoint:c_call `puts'@trace.rb:2>
#<TracePoint:c_call `puts'@trace.rb:2>
#<TracePoint:c_call `write'@trace.rb:2>
1#<TracePoint:c_return `write'@trace.rb:2>
#<TracePoint:c_call `write'@trace.rb:2>

#<TracePoint:c_return `write'@trace.rb:2>
#<TracePoint:c_return `puts'@trace.rb:2>
#<TracePoint:c_return `puts'@trace.rb:2>
#<TracePoint:return `func_1'@trace.rb:3>
{% endhighlight %}

## (2) Ruby VM
> Ruby1.9使用yarv虚拟机，可以通过yarv类了解代码的中间代码
{% highlight ruby %}
code = RubyVM::InstructionSequence.compile('a = 1; puts 1 + a')
puts code.disassemble
>>>>
== disasm: <RubyVM::InstructionSequence:<compiled>@<compiled>>==========
local table (size: 2, argc: 0 [opts: 0, rest: -1, post: 0, block: -1] s1)
[ 2] a
0000 trace            1                                               (   1)
0002 putobject_OP_INT2FIX_O_1_C_
0003 setlocal_OP__WC__0 2
0005 trace            1
0007 putself
0008 putobject_OP_INT2FIX_O_1_C_
0009 getlocal_OP__WC__0 2
0011 opt_plus         <callinfo!mid:+, argc:1, ARGS_SKIP>
0013 opt_send_simple  <callinfo!mid:puts, argc:1, FCALL|ARGS_SKIP>
0015 leave
{% endhighlight %}

## (3) 分布式Ruby, drb
{% highlight ruby %}
#server.rb
require 'drb'
class TestServer
  def add(*args)
    args.inject {|n,v| n + v}
  end
end
server = TestServer.new
DRb.start_service('druby://localhost:9000', server)
DRb.thread.join # Don't exit just yet!

# client.rb
require 'drb'
DRb.start_service()
obj = DRbObject.new(nil, 'druby://localhost:9000') # Now use obj
puts "Sum is: #{obj.add(1, 2, 3)}"

>>>> ruby client.rb
Sum is: 6
{% endhighlight %}

## (4) Ruby安全控制
> NOTE1: Ruby通过$SAFE变量定义代码执行的安全等级；
> NOTE2: Ruby可以通过Object#tainted?方法来判断对象是否已经被`污染`；
> NOTE3: Ruby可以通过trusted?或者untrusted来判断对象是否信任；(ruby1.9+)

## (5) 标准转换协议
{% highlight ruby %}
to_ary
to_a
to_enum
to_hash
to_int
to_io
to_open
to_path
to_proc
to_regexp
to_str
to_sym
{% endhighlight %}

## (6) to_proc, coercise vs performance
{% highlight ruby %}
# performance better
names = %w{ant bee cat}
result = names.map {|name| name.upcase}

# elegant better
names = %w{ant bee cat}
result = names.map(&:upcase)

########
require 'benchmark'
Benchmark.bm(20) do |b|
  b.report("coecrise") {['a', 'b'].map(&:upcase)}
  b.report("standard") {['a', 'b'].map{|x| x.upcase} }
end
>>>>>>>
ruby performance.rb
                           user     system      total        real
coecrise               0.000000   0.000000   0.000000 (  0.000011)
standard               0.000000   0.000000   0.000000 (  0.000006)

{% endhighlight %}

> map的传入参数是一个block，如果本身不是proc，Ruby会自动调用to_proc方法
> Ruby可以自动根据symbol转换为proc

## 双分派模式(double dispatch)
> coerce方法根据自己的类型，以及传入参数的class来决定到底生成什么对象；
{% highlight ruby %}
puts 1.coerce(2.0)
puts (3.0).coerce(1)
puts 1.coerce(2)
>>>>>
2.0
1.0
1.0
3.0
2
1
{% endhighlight %}
## 语言特性
> TRICK1: 支持管道式编程
{% highlight shell %}
echo 'puts "Hello"' | ruby
{% endhighlight %}
> TRICK2: Ruby中分为Integer与Bignumber两种表示数字的class，Integer表示范围受到计算机位数的限制，BigNum则受到机器内存的限制，两种可以自动转换；
> TRICK3: Ruby1.9的正则表达式引擎：Oniguruma, 不是Perl的正则表达式引擎；Ruby2.0使用Onigmo引擎，为1.9的升级版本，在正则的一些模式上有所扩展，表达能力更强；
> TRICK4: block是一段代码块，不是对象，需要绑定一个方法；Proc是一个对象，可以调用call方法；


















