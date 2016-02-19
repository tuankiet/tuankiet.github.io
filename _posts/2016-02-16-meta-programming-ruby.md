---
layout: post
title: "Meta programming in Ruby"
date: 2016-02-16
categories: ruby
---

> Metaprogramming - writing code writes code itself.

Metaprogramming is a technique, by which, you can write code that writes code itself dynamically at runtime.
You can define methods, classes at during runtime and you can reopen predefined class, modify, add methods, catch methods if don't exist, create them at runtime.
Metaprogramming make code clean and DRY.

You must know in Ruby:

  - understanding `self`
  - class methods
  - instance methods
  - singleton methods

> Everything in Ruby is a Object, Every behaviors is a methods call

Example for reopen class, override methods:

{% highlight ruby %}
class Foo
  def say_hello
    puts "hello!"
  end
end
foo = Foo.new
foo.say_hello
=> "hello!"

class Foo
  def say_hello
    puts "goodbye!"
  end
end
foo.say_hello
=> "goodbye!"
{% endhighlight %}

Example for singleton methods:

{% highlight ruby }
class Foo
end
foo = Foo.new

def foo.say_hello
 p "hello!"
end

foo.say_hello
=> "hello!"
puts Foo.new.repond_to?(:say_hello)
=> false
{% endhighlight }

> `class << self` meaning?

Let's see example code:

{% highlight ruby %}
class Developer

  def frontend
    p "inside instance method, self is: " + self.to_s
  end

  class << self
    def backend
      p "inside class method, self is: " + self.to_s
    end
  end

end
{% endhighlight %}

create instance of `Developer`: `dever = Developer.new`

call instance methods: `dever.frontend => 'inside instance method, self is: #<Developer:0x00000003321538>'`

call singleton methods or class methods, `:backend is singleton methods of class Developer`: `Developer.backend => 'inside class method, self is: Developer'`

You can show instance methods of one Object by using `:instance_methods false`

`p dever.class.instance_methods false` => '[:frontend]'

`p developer.class.singleton_class.instance_methods false` => '[:backend]'

Summary, `class << self` syntax used to opens up self's singleton class

Additional, They can use `class << self` to Grouping class methods in specify Class, to identify methods is a class methods.

> Defining methods using `class_eval` and `instance_eval`

`instance_eval` to create class methods, see example:

{% highlight ruby %}
class Developer; end
Developer.instance_eval do
  p "instance_eval - self is:" + self.to_s
  def backend
    p "inside a method self is: " + self.to_s
  end
end
# "instance_eval - self is: Developer"

Developer.backend
# "inside a method self is: Developer"
{% endhighlight %}

On the other hand, `class_eval` evaluates the in the context of a class of an instance. It reopens the class.
Here is how `class_eval` create instance method:

{% highlight ruby %}
Developer.class_eval do
  p "class_eval - self is: " + self.to_s"
  def frontend
    p "inside a method self is: " + self.to_s
  end
end
# "class_eval - self is: Developer"

p developer = Developer.new
# #<Developer:0x2c5d640>

developer.frontend
# "inside a method self is: #<Developer:0x2c5d640>"
{% endhighlight %}

> Defining method on the Fly

In Metaprogramming, when we call methods on one object, Ruby first goes into class of object and browsing this methods, it missing, Ruby browsing in superclass. If Ruby still doesn't find the method, it call another method named `method_missing` which instance method of `Kernel` class (everything is inherit this).

We can define method dynamic by using method `define_method`, see example:

{% highlight ruby%}
class Developer
  define_method :frontend do |*args|
    p "inside frontend instance method"
  end

  class << self
    def create_backend
      singleton_class.send(:define_method, "backend") do
        "inside create_backend method"
      end
    end
  end
end
dever = Developer.new
dever.frontend
# "inside frontend instance method"

p Developer.backend
# undefined method 'backend'

Developer.create_backend
p Developer.backend
# inside create_backend method
{% endhighlight %}

This code below is similar, it make DRY, define 2 instance method frontend and backend:

{% highlight ruby %}
cass Developer

  ["frontend", "backend"].each do |method|
    define_method "coding_#{method}" do
      p "writing " + method.to_s
    end
  end

end
{% endhighlight %}
