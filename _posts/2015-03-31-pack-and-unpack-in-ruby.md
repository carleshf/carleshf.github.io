---
published: true
title: pack and unpack in ruby
category: blog
layout: post
author: carleshf
tags:
  - ruby
---

I still enjoy how complex operations become simple when using ruby:

```ruby
module Converter
  def self.to_ascii(hex)
    return [hex].pack("H*")
  end

  def self.to_hex(ascii)
    return ascii.unpack('H*')[0]
  end
end
```