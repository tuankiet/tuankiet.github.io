---
layout: post
title: "Ruby interview questions"
date: 2016-03-03
categories: ruby
---

1. Precendence

```ruby
val1 = true and false
val2 = true && false
```

=> This question about Precedence. `and or` have lower precedence than `=`. `&& ||` have higher precedence than `=`

```ruby
(val1 = true) and false
val2 = (true && false)
```

2. False or True

```ruby
true    ? "true" : "false"
false   ? "true" : "false"
nil     ? "true" : "false"
1       ? "true" : "false"
0       ? "true" : "false"
"false" ? "true" : "false"
""      ? "true" : "false"
[]      ? "true" : "false"
```

=> `false` `nil` evaluate to false. Everything else even Zero or emty array evaluates to true.

3. Write function sort the keys of hash by the length of keys.

```ruby
hash.keys.map(&:to_s).sort_by(&:length)
```

4. Difference `super` and `super()`

`super` invoke the parent method with the same arguments pass in the child method if don't match raise error.

`super()` invoke the parent method without any arguments

continue updated : (link)[https://www.toptal.com/ruby/interview-questions] and (link)[https://gist.github.com/ryansobol/5252653]
