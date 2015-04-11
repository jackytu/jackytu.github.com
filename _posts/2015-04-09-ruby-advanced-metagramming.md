---
layout: post
title: "ruby programming advanced - metagramming"
description: "ruby programming advanced"
category: ruby
tags: [ruby]
---
{% include JB/setup %}


## (1) Metaprogramming
### 1.1 关于self
> 1. ruby中每次方法调用都需要绑定到一个对象(receiver)；
>
> 2. 对于没有显示指定object的方法调用，例如 puts 'echo',ruby会将self置为当前的object，并从当前类开始搜索，直到BasicObject类；
>
> 3. Ruby程序运行的过程中，会不断改变self的值；

### 1.2 类方法(Notion)
{% highlight ruby %}
#example
duck = "Donald Duck"
#M1.2.1
def duck.speak
  puts "I'm Donald"
end
duck.speak

>>>
I'm Donald
{% endhighlight %}

 > 误区: 按照ruby的基本语义，duck.speak方法调用时，会将self设置为duck对象，再从String类开始搜寻speak方法，但显然String中没有speak方法，why?
 > NOTE: ruby并没有'类方法'概念,所有的方法均是实例方法！
 > M1.2.1 其实会自动创建一个匿名类，其祖先关系为: duck(object) < 匿名类 < String，这也是一个单例类，speak方法也成为单例方法，只能被新创建的匿名类调用，因此，这里的调用会先到匿名类中找speak方法，从而成功完成这次调用；

{% highlight ruby %}
singleton = class << "cat"; self; end
singleton.new #这里将会报错，因为单例类不能实例化
{% endhighlight %}

 > NOTE：attr_accessor访问class object的实例变量
{% highlight ruby %}
# 这个例子可以像一般的实例属性操作一样，操作class-object的实例变量
class Cls
  @var = 'old'
  class << self
    attr_accessor :var
  end
end
puts Cls.var
Cls.var = 'new'
puts Cls.var

>>>
old
new
{% endhighlight %}

> NOTE: ruby中需要理解object的class与其object.class的class之间的关系与区别
{% highlight ruby %}
class Cls
  def self.a_method
    puts "hello"
  end
end

obj = Cls.new
# 这句代码将成功执行，因为ruby可以在其ancestors中Anno中找到a_method的定义；
# 其祖先链为[Anno, Class, Module, Object, Kernel, BasicObject]
Cls.a_method
puts Cls.ancetors

# 这句话将执行失败，因为a.class(Cls)的ancestors中找不到a_method的定义；
# Cls的祖先链路为[Object, Kernel, BasicObject]
a = Cls.new
a.a_method

{% endhighlight %}

### 1.3 可见性
> TRICK: ruby的子类可以修改父类方法的权限
{% highlight ruby %}
class A
  def a_method
    puts "parent"
  end
  private :a_method
end
class B<A
  public :a_method
end
b = B.new
b.a_method

>>>>
parent
{% endhighlight %}

### 1.4 Mixin
> NOTE1: 用include导入一个Module，Ruby将生成一个**匿名类**，插入到当前类与其父类之间；
> NOTE2: include导入的模块，**匿名类**会增加一个到Module的reference，也就是说只要模块发生变化，引用该模块的类的行为都会相应发生变化；
{% highlight ruby %}
module Mod
  def greeting
    "Hello"
  end
end

class Example
  include Mod
end

ex = Example.new
puts "Before change, greeting is #{ex.greeting}"

module Mod
  def greeting
    "Hi"
  end
end
puts "After change, greeting is #{ex.greeting}"

>>>
Before change, greeting is Hello
After change, greeting is Hi
{% endhighlight %}

>>> NOTE3: 如果需要在类中既mixin实例方法，又mixin类方法，可以使用self.included方法
>>> NOTE4: 与self.included方法类似，Ruby定义了很多类似的hook，比如method_added，
>>>        coerce, induced_from, 等等.
{% highlight ruby %}
module Mod
  def ins_method
    puts "ins"
  end

  module ClsMethod
    def attr_maker(attr)
      attr_reader :attr
      define_method("#{attr}=") do |val|
        instance_val_set("@#{attr}", val)
      end
    end
  end

  def self.included(cls)
    cls.extend(ClsMethod)
  end
end

class Cls
  include Mod
  attr_maker :attr
end

obj = Cls.new
obj.attr = 1
puts obj.attr

>>>
1
{% endhighlight %}

### 1.5 {instance,class}_{eval,exec}
> _eval函数只能跟block，不能带参数，_exec函数则可以；
{% highlight ruby %}
# instance_eval无法传递变量
@a = 1
"cat".instance_eval do
  puts @a
end
>>>
#空

# instance_exec可以传递变量
@a = 1
"cat".instance_exec(@a) do |b|
   puts b
end
>>>
1
{% endhighlight %}
> instance_{eval,exec}将生成`类方法`，class_{eval,exec}将生成实例方法；
> TRICK: 利用instance_eval/instance_exec很容易实现DSL，可以不用定义receiver，因此，看上去可以非常简洁；
> TRICK：但是在使用instance_eval时，需要注意实例变量的作用域;
{% highlight ruby %}

puts "self1 #{self}"
"cat".instance_eval do
  PUTS "self2 #{self}"
end

# 这里self1 != self2
{% endhighlight %}

> TRICK：eval可以通过binding自定义上下文
{% highlight ruby %}
def context1
  val = 'in_context1'
  binding
end

def context2
  val = 'in_context2'
  eval("val; puts val", context1)
end

context2

>>>>
in_context1
{% endhighlight %}

### 1.6 method/send
> method与send让动态方法调用成为可能;
> method类似proc，或者C++中的指针,在需要的时候调用call即可;
> 与method类似的有proc,lamdba等，可以实现动态调用；
{% highlight ruby %}
cat_sub = "cat".method(:sub)
puts cat_sub.call('c', 'b')
>>>>
bat
{% endhighlight %}

> TRICK: method的动态绑定
{% highlight ruby %}
class A
  def func
    puts "IN A"
  end
end

class B<A
  def func
    puts "IN B"
  end
end

class C<B
  def func
    puts "IN C"
  end
end

c = C.new
# call func of C
c.func
# call func of B
B.instance_method(:func).bind(c).call
# call func of A
A.instance_method(:func).bind(c).call
>>>>
IN C
IN B
IN A
{% endhighlight %}
### 1.7 其他
> TRICK1: tap调试与简化代码
{% highlight ruby %}
# 在函数栈比较深的时候，可以用tap进行debug
arr = (1...10).tap{|x| puts "x is #{x.inspect}"}
  .to_a.tap{|x| puts "x is #{x.inspect}"}
  .select{|x| x%2 == 0}.tap{|x| puts "x is #{x.inspect}"}

puts arr
>>>>>
2
4
6
8

# trace 系统命令的执行
class Object
  old_system_method = instance_method(:system)
  define_method(:system) do |*args|
    old_system_method.bind(self).call(*args).tap do |result|
      puts "system(#{args.join(', ')}) returned #{result.inspect}"
    end
  end
end

system("date")

>>>>
2015年 4月11日 星期六 11时06分31秒 CST
system(date) returned true
 {% endhighlight %}
>>> NOTE: ruby顶层文件的执行环境其实是Object，def一个method，其实是定义Object的instance_methods;

## (2) Duck-Typing
> Hint: Duck-Typing的关键是什么？
  * 与其他强类型的语言不同，ruby的class并不是==对象的类型==;
  * class更加强调object能够做什么，而不是object是什么；
  * 经典谚语：`walk like a duck, then it's a duck`
  * duck-typing的好处可以让程序员省去大量校验类型的代码；











