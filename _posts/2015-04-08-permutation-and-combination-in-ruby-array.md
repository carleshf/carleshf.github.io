---
published: true
category: blog
layout: post
title: Permutation and combination in ruby's arrays
---

Today I was working on a [simple kata](http://www.codewars.com/kata/54381f0b6f032f933c000108/train/ruby) of CodeWars that required to work with combinations and permutations of elements given in array.

The number of __permutations__ of the __n__ elements in a set taken by groups of __k__ is given by:

$latex
\frac{n!}{(n-k)!} &s=4
$

In this case, the order within the groups matters. If order does not matter, then we are talking about __combinations__:

$latex
\frac{n!}{k!(n-k)!} = \binom{n}{k} &s=4
$

The `array` class from ruby comes with a method (`permutation`) to get the permutations of its elements given a value __k__:

```ruby
[1,2].permutation(0).to_a
=> [[]]
[1,2].permutation(1).to_a
=> [[1], [2]]
[1,2].permutation(2).to_a
=> [[1, 2], [2, 1]]
[1,2].permutation(3).to_a
=> []
```

In the same way, the class `array` comes with a method `combination` to get the combinations of its elements given a value __k__:

```ruby
[1,2].combination(0).to_a
=> [[]]
[1,2].combination(1).to_a
=> [[1], [2]]
[1,2].combination(2).to_a
=> [[1, 2]]
[1,2].combination(3).to_a
=> []
```

So with these two methods we can get the sets results of permuting and combining the elements of an array but not to calculate the elements of a k-permutation of k-combination. 

To do that I propose:

 1. To include the method `factorial` to class `Fixnum`.
 2. To include the methods `perm_length` and `comb_length` to class `array`

These two steps can be done as:

```ruby
class Fixnum
  def factorial
    f = 1
    (1..self).each{|ii| f *= ii }
    return f
  end
end

class Array
  def perm_length k
    return self.length.factorial / (self.length - k).factorial
  end
  def comb_length k
    return self.length.factorial / (k.factorial * (self.length - k).factorial)
  end
end
```

Something more complex would be needed for using this in a real scenario like a control for negative values of __k__. But for me it's just enough :-)