---
published: true
layout: post
title: Unload (detach) a loaded R package
---

A long the process of creating and testing R packages I usually need to *unload* a loaded package. The function to perform this operation is called `detach` and must be run as:

```R
detach(name="package:rexposome", unload=TRUE)
```

The `name` argument needs to be filled with `package:` and followed by the name of the package to be unload, that can be found at `search()`.
