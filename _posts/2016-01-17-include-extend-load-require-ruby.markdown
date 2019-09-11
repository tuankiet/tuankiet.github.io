---
layout: default
comments: true
title: "Include vs Extend. Require vs Load"
date: 2016-01-17
categories: ruby
---

# Include vs Extend, mixing functions into class

In `Class` where include or extend appear:
- Include will create `instance methods`.
- Extend will create `class methods`.

{% highlight ruby%}
module Example
  def say_hello
    puts "hello!"
  end
end

class TestExample
  include Example
end

test = TestExample.new
test.say_hello
# "hello!"

class TestExample
  extend Example
end

TestExample.say_hello
# "hello!"
{% endhighlight %}

# Load vs Require

Load and Require often put in top of class or module. They have same feature: load somethings from other file into destination file.

Load:
- it doesn’t keep track of whether or not that library has been loaded
- load library mutiple times
- must specify the ".rb" extension of the library file name
- ex: `load 'test_library.rb'`

Require:
- load library and prevents it from being loaded more than one
- return `false` if you try require multiple one library
- don't need extension ".rb"
- put on very top of file
- ex: `require 'test_library'`

