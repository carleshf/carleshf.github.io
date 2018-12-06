---
published: true
category: blog
layout: post
title: Decompress all files in folder
author: Carles Hernandez-Ferrer
date: '2015-11-01'
slug: decompress-all-files-in-folder
categories:
  - tools
  - bash
tags:
  - zip
---

Just a line to decompress all the `.fastq.gz` files in a folder:

```bash
for file in $(ls | grep gz); do gzip -d $file; done
```

