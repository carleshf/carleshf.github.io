---
published: true
title: Procedural generation of abstract glyphs
category: blog
layout: post
author: carleshf
tags:
  - haxe
  - openfl
---

Some days ago I randomly ended into the subreddit on __Procedural generation__, which I didn't suspect it existed. In specific, I ended up into the [_monthly challange_](https://www.reddit.com/r/proceduralgeneration/comments/5wzo7j/monthly_challenge_16_march_2017_procedural_runes/) of March 2017.

As you can see there, the challenge was on generating a set of _runes_, _glyphs_, or _symbols_. I found the challenge very interesting and stating from one of the pictures they reference ([this one](http://i.imgur.com/haZhAVz.png)) I started my own _generator_ as an excuse to learn a bit of [Haxe](https://haxe.org/) and [OpenFL](https://www.openfl.org/).

I focused on the _gliphs_ with triangular forms, the ones called "actions" in the reference picture. I first sifted the _gliphs_ from the figure on order to have a more clear picture of the type of _triangular gliphs_ I wanted to create. So, I removed "connect" and "reflect" since they broke the pyramidal structure of the others ones, and I also sifted "learn", "understand", and "protect", because they added two new elements (the circle and the dot) that I dislike.

So I started by placing a triangle in a matrix of 9x9 dots and assigning a probability of 0.9 to a full closed triangle and a probability of 0.1 to a base-open triangle.

![Outline of triangle glyphs]({{baseurl}}/assets/haxe-triangle-glyph-01.png)

Then I dissected the glyph and obtained ten small pieces with which to create the filling of the triangle and create a _glyph_. I assigned the same probability to each piece to be part of the final _glyph_.

![Content of triangle glyphs]({{baseurl}}/assets/haxe-triangle-glyph-02.png)

The last part was to decide the colors to use as background and for the _glyphs_. I went for a _old-paper_ color as background and some _old-ink_ colors for the _glyphs_.

![Colors for triangular glyphs]({{baseurl}}/assets/haxe-triangle-glyph-03.png)

In order: _paper_ - `0xfff8db`, _light salmon_ - `0xf78bc1`, _cloud blue_ - `0xafd8df`, and _ink blue_ - `0x000f55`.

The final piece can be in in [my dashboard](https://carleshf.itch.io/) at itch, just look for "Triangle Glyphs". The code is available in [GitHub](https://github.com/carleshf/triangleGliph).