---
layout: post
title: "ruby"
description: "ruby programming advanced - reflection"
category: ruby
tags: [ruby, reflection]
---
{% include JB/setup %}


## (1) Reflection
### 1.1 object
> 1. Ruby可以使用ObjectSpace#each_object来获取当前空间类所有的object;
> 2. Ruby可以通过object_id获取对象的ID；
{% highlight ruby %}
num = 1
puts num.object_id

>>>>
70235424456840
{% endhighlight %}

### 1.2 Class
> 1. Ruby提供了多种访问Class内部信息的方法：
{% highlight ruby %}
class Demo; self; end
Demo.private_instance_methods(false)
Demo.protected_instance_methods(false)
Demo.protected_instance_methods(false)
Demo.singleton_methods(false)
Demo.class_variables
Demo.constants(false)
demo = Demo.new
demo.instance_variables
demo.public_method
demo.instance_variables
{% endhighlight %}


