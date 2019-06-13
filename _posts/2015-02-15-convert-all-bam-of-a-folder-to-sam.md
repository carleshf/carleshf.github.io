---
published: true
title: Convert all .bam of a folder to .sam
category: blog
layout: post
author: carleshf
tags:
  - bash
  - bioinformatics
---

The following is the bash command I use to convert all the `BAM` files in a folder to `SAM` files using `SAMtools`:


```bash
for file in ./*.bam; do
    echo $file 
    ~/Software/samtools-0.1.19/samtools view -h $file &gt; ${file/.bam/.sam}
done
```
