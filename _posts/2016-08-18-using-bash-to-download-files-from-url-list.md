---
published: true
title: Using bash to download files from a list of URLs
category: blog
layout: post
author: carleshf
tags:
  - bash
---

Today I need to download a large list of files. So I put all the URLs in a file (called `url`) and iterate of it in bash to download them by `wget`:

```bash
#!/usr/bin/env bash

while read ll; do
        wget $ll
done &lt; url
```

