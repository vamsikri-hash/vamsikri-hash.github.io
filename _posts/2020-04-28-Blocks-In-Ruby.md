---
layout: post
title: Blocks In Ruby
date: 2020-04-28
---

In this post I will try to explain what blocks are in ruby which is most oftenly used feature of ruby.

Let's have a look at this piece of code.

```ruby
[1,2,3,4].each { |i| puts i }
```

one may guess that it will go through the array and print each element. When we observe the syntax.it is clear that `each` is a method called on `array` then what about code with curly braces . You may have doubt that, Is this an argument to each? How this works? so before going to know about that let us know about blocks.

What we have seen above in curly brace just after the method is called blocks in ruby.

A block is a piece of code just like methods but do not have a name. A block can take arguments, and process the code inside it. It can be viewed as anonymous functions in some programming languages.
General usage of blocks in ruby is, it can be implicity or explicitly passed to methods and can be helpful in running some code inside the methods.

Now let's take an example, with a method called caluculator.

```ruby

def caluculator(x,y)
  puts "entered into method"
  yield(x,y)
  puts "outside the block"
end

caluculator(4,5) { |x,y| puts x+y }

#  output of above line
#  entered into method
#  9
#  outside the block

```

The method caluculator takes two parameters x,y and a implicit block. In the above example it takes 4,5 and prints 9. Where we have given that logic inside a block. so when we pass a block to a method it is going to execute inside the methods. But how does the method knows to run a block. The magic is in `yield`. yield is a special keyword in ruby (not a method, ofcourse takes some arguments). whenever we use `yield` it is going to call the block and execute that inside a method.

A block can take arguments as well , we take those in between two vertical bars (`| |`). Whatever the arguments you pass to block need to be passed through `yield` so that it can be available inside the block.

An important point to note, `yield` should be used only when you pass a block otherwise it will raise an error `no block given`. But we can handle this as well without getting error, by using a method `block_given?`. It checks whether a block is passed to method or not and return the boolean according to it( true if present).

```ruby
def caluculator(x,y)
  puts "entered into method"
  if block_given?
    yield(x,y)
  else
    puts "No block is given."
  end
  puts "outside the block"
end

caluculator(4,5)

# output
# entered into method
# No block is given.
# outside the block

```

In above case if block is not given , it can be handled within else section.

> ## syntax of blocks

A block can also written with in `do end` inplace of curly braces. General convention is to write the single line code within `{ }` and multi-line code with in `do end`.

```ruby
caluculator(4,5) do |x,y|
   puts x+y
end

# output is same as the above method (caluculator(4,5) { |x,y| puts x+y })

```

> ## Usage of blocks in builtin methods

In ruby we frequently use methods like each , each_with_index, map and select to iterate or manipulate over the array etc. when you observe, we always use a block along with those methods like you saw in the first piece of code at the start of post. we can also write those methods on our own , let us write a proxy of each method for arrays .

The above mentioned methods like each, select are written inside the [Enumerable module](https://ruby-doc.org/core-2.7.1/Enumerable.html). so let's write our method also inside this , so we can use this on an array.

```ruby
module Enumerable
  def my_each
    if block_given?
      len = self.length
      index = 0
      while index < len
        yield(self[index])
        index += 1
      end
      self
    else
      puts "No block is given."
    end
  end
end


[1,2,3,4].my_each { |i| puts i+2 }

# output
# 3
# 4
# 5
# 6

[1,2,3,4].my_each

# output (without a block)

#  No block given.
```

Here `my_each` method can be used as each . In above method `self` is nothing but the array which we invoke the `my_each` method. Here it simply iterate over the each element in array and pass that to block using `yield`.So here block takes that each element as argument and process the code inside it. This is simple example to show how blocks are helpful in ruby.

This is simple about blocks . But there is much to know about it. Here is [link to rubymonk's blocks section](https://rubymonk.com/learning/books/4-ruby-primer-ascent/chapters/18-blocks/lessons/51-new-lesson) where you can learn more about it.
