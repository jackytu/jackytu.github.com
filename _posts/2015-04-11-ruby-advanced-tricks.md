---
layout: post
title: "ruby tricks"
description: "ruby programming advanced - tricks"
category: ruby
tags: [ruby, tricks]
---
{% include JB/setup %}


## (1) Trace技巧
> TRICK1: ruby2.0可以使用TracePoint来进行调用trace;
> TRICK2: ruby1.9则可以使用caller方法
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

