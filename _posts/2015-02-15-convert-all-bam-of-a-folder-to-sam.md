---
published: true
layout: post
title: Convert all .bam of a folder to .sam
author: Carles Hernandez-Ferrer
date: '2015-02-15'
slug: convert-all-bam-of-a-folder-to-sam
categories:
  - bash
  - bioinformatics
tags:
  - bam
  - sam
---

The following is the bash command I use to convert all the `BAM` files in a folder to `SAM` files using `SAMtools`:


```bash
for file in ./*.bam; do
    echo $file 
    ~/Software/samtools-0.1.19/samtools view -h $file &gt; ${file/.bam/.sam}
done
```
