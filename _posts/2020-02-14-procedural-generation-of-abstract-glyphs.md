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

As you can see there, the challenge was on generating a set of _runes_, _glyphs_, or _symbols_. I found the challenge very interesting and stating from one of the pictures they reference ([this one](http://i.imgur.com/haZhAVz.png)) I decided to create my own _generator_ as an excuse to learn a bit of [Haxe](https://haxe.org/) and [OpenFL](https://www.openfl.org/).

I focused on the _gliphs_ with triangular forms, the ones called "actions" in the reference picture. I first sifted the _gliphs_ from the figure in order to have a more clear picture of the type of _triangular gliphs_ I wanted to create. So, I removed "connect" and "reflect" since they broke the pyramidal structure of the others ones. I also sifted "learn", "understand", and "protect", because they added two new elements (the circle and the dot) that I dislike.

I started by placing a triangle in a matrix of 5x5 dots. Then I assigned a probability of 0.9 to a close-base triangle and a probability of 0.1 to a open-base triangle.

![Outline of triangle glyphs]({{baseurl}}/assets/haxe-triangle-glyph-01.png)

With the border already working, I dissected the original _glyph_ and obtained ten small pieces with which to create the filling of the triangle to create a _glyph_. I assigned the same probability to be part of the final _glyph_ to each piece.

![Content of triangle glyphs]({{baseurl}}/assets/haxe-triangle-glyph-02.png)

After many test, I decided to impose some rules to avoid the creation of overcharged _glyps_:

   1. A _glyph_ will have a maximum of a border and three pieces with a probability of 0.6 to have a single piece, 0.3 for two pieces, and 0.1 for three pieces.
   2. When added any diagonal lines none of the following pieces can be added (and vice versa): (small) happy face, (small) sad face, leg (line in the middle), (large) sad face, nor any diagonal.
   3. When added (small) sad face we cannot add large sad face (and vice versa).
   4. Belt line (horizontal mid line within the triangle) cannot be combined with lance line (horizontal mid line across the triangle).
   5. Ceiling (horizontal line on the top of the _glyph_) and lance cannot be combined.

With this rules in place, I liked the type of _glyphs_ obtained from the random generation.

The last part was to decide the colors to use as background and for the glyphs. I went for a old-paper color as background and some old-ink colors for the glyphs:

   * _paper_: `0xfff8bd`
   * _pink salmon_: `0xf78bc1`
   * _cloud blue_: `0xafd8df`
   * _ink blue_: `0x000f55`

![Colors for triangular glyphs]({{baseurl}}/assets/haxe-triangle-glyph-03.png)

The final piece can be seen in [my dashboard](https://carleshf.itch.io/) at itch (just look for "Triangle Glyphs"). The small web application allows you to generate a wall with 30 randomly generated _glyps_, an alphabet with 25 unique _glyps_, and to see the pieces used to create all of them.

The code is available at [GitHub](https://github.com/carleshf/triangleGliph).
