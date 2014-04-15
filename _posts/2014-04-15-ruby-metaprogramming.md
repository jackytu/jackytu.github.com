---
layout: post
title: "ruby metaprogramming"
description: ""
category: ruby
tags: [ruby, metagramming]
---
{% include JB/setup %}

##参数解析的栗子
在大型系统开发中，经常需要提取上游模块传输的数据，封装成一个本模块需要处理的数据结构，比如：
需要提取一个数据包里面的id与内存使用数据，封装成另外一个数据结构，按照不用思考的逻辑来实现，
你的代码可能会是：


```ruby
2.0.0-p451 :009 > input = { "id" => "123", "memory" => "128MB", "port" => 12345 }
 => {"id"=>"123", "memory"=>"128MB", "port"=>12345}
2.0.0-p451 :010 > output ||= {}
 => {}
2.0.0-p451 :011 > output['id'] = input['id']
 => "123"
2.0.0-p451 :012 > output['memory'] = input['memory']
 => "128MB"
2.0.0-p451 :013 > p output
{"id"=>"123", "memory"=>"128MB"}
 => {"id"=>"123", "memory"=>"128MB"}

```

上述代码的问题在于，你需要为每个你关心的字段写一行代码，是不是显得很冗余？


作为有理想的IT攻城⑩，，肯定无法忍受如此无聊的重复劳动，就让元编程技术来解救我们吧！


```ruby
2.0.0-p451 :023 > input = { "id" => "123", "memory" => "128MB", "port" => 12345 }
 => {"id"=>"123", "memory"=>"128MB", "port"=>12345}
2.0.0-p451 :024 > output = {}
 => {}
2.0.0-p451 :025 > ["id", "memory"].each do |key|
2.0.0-p451 :026 >     output[key] = input[key]
2.0.0-p451 :027?>   end
 => ["id", "memory"]
2.0.0-p451 :028 > p output
{"id"=>"123", "memory"=>"128MB"}
 => {"id"=>"123", "memory"=>"128MB"}
```

看到了没，你可以将你需要关心的属性列到一个数组中，然后对这些字段做通用的操作即可，是不是
简单很多？


##send
##define_method


