---
published: true
title: Decompress all files in folder
category: blog
layout: post
author: carleshf
tags:
  - bash
---

Just a line to decompress all the `.fastq.gz` files in a folder:

```bash
for file in $(ls | grep gz); do gzip -d $file; done
```

