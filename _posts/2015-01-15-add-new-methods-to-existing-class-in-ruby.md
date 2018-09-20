---
published: true
layout: post
title: Add new methods to existing classes in ruby
---

I was working on a _kata_ from [CodeWars](http://www.codewars.com) that asks to implement a series of methods that involve working with arrays. This led me to discovered a feature from ruby language that impressed me: You can extend the functionality of an existing class! In other words, you can add methods to an existing class.

To extend the functionality of `Array` class with two methods that allows to get the odds and evens number within the array we can do:

```ruby
class Array

  def even
    self.select{|x| x % 2 == 0}
  end
  
  def odd
    self.reject{|x| x % 2 != 0}
  end

end
```

And now, the object from `Array` will have both methods:

```ruby
> [1, 2, 3, 4, 5].even
=> [2, 4]
> [1, 2, 3, 4, 5].odd
=> [1, 3, 5]
```