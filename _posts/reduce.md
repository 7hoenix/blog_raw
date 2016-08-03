---
layout: post
title: Reduce in 3 languages
author: Jphoenx
---

## What is reduce?

Reduce is a way to iterate through a collection and bring it down to a single result by applying the given function to it.

Ruby:

```
class Array
  def my_reduce(starting = nil, &my_block)
    if !starting
      starting = self.shift
    end

    self.each { |element| starting = my_block.call(starting, element) }
    starting
  end
end

my_collection = [1, 2, 3, 5, 4, 16, 1000]
add_just_odd_ones = lambda { |acc, element| element % 2 == 1 ? acc + element : acc }
add_values = lambda { |acc, element| acc + element }

my_collection.my_reduce(&add_just_odd_ones)
=> 9

my_collection.my_reduce(&add_values)
=> 1031
```

A few notes in case you aren't super familiar with Ruby:

We are **open classing** the Array class and extending it with our own method **my_reduce**.
That way we can just make an array and call our reduce method directly on it (just like with
the regular reduce method).

lambda's in Ruby are similar to functions in other languages. You declare them and then can
pass them around with the & symbol.
Then when you are ready to call them simply invoke the method .call on the lambda.


