---
published: true
layout: post
category: blog
title: R and regex - find all occurrences
---

In R there are many functions that work with a pattern written as a regular expression. Today I needed to deal with one of these functions: `str_locate_all` ([doc](http://www.rdocumentation.org/packages/stringr/versions/1.1.0/topics/str_locate)) from `stringr`

My goal was to find `"223777_at [Chip: U133B]"` in a series of strings like the following one:

```R
text <- "11753227_s_at [Chip: PrimeView]; 223777_at [Chip: HT_HG-U133B]; 223777_PM_at [Chip: U133_Plus_PM]; 48336_at [Chip: U95B]; 223777_at [Chip: GeneProfilingArray]; g13477210_3p_at [Chip: U133_X3P]; MmugDNA.4759.1.S1_at [Chip: Rhesus]; 11753227_s_at [Chip: HG-U219]; ADXECADA.19261_s_at [Chip: Xcel]; ADXECRS.13279_at [Chip: Xcel]; ADXECRS.13279_x_at [Chip: Xcel]; 223777_at [Chip: U133B]; 223777_at [Chip: U133_Plus_2]; RC_T49570_at [Chip: Hu35KsubB]"
```

The way to find the location (in my case all the locations) of the pattern _number slash at white-space bracket Chip two-points U133B brecket_ follows:

```R
str_locate_all(text, pattern="[0-9]+_at \\[Chip: U133B\\]")
```

This returns a `list` of `matrix` with the locations (start and end point) of all the occurrences found by the given pattern. __Take care__, so the brackets need to be __escaped by double slash__.
