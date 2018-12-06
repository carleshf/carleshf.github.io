---
published: true
layout: post
category: blog
title: Extract paired-end reads from (NCBI) SRA files
author: Carles Hernandez-Ferrer
date: '2016-01-24'
slug: extract-paired-end-reads-from-ncbi-sra-files
categories:
  - tools
  - bioinformatics
tags:
  - NCBI
---

[SRA](http://www.ncbi.nlm.nih.gov/sra) stores all the sequencing from [GIO](http://www.ncbi.nlm.nih.gov/geo/) experiments in files in `.sra` format. These files are managed using the [SRA Toolkit](http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc).

I recently download some `.sra` files from this [GEO](http://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM1616493) corresponding to paired-end sequencing data. My surprise when I run [`fastq-dump`](http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&amp;f=fastq-dump) (from SRA toolkit) utility and I got only one file rather than two.

From the documentation of the tool, it seems that the option `--split-files` should be enough but not. We need to add the `--split-3` option. If we run `fastq-dump` with this configuration in a single-end experiment a single `.fastq` files will be create, otherwise two files with suffixes `_1` and `_2` will be the matched paired read files (`.fastq`) while a posible third file (no sufix) will contain the non matched reads.

I currently run `fastq-dump` as:

```
fastq-dump --split-files --split-3 SRR1813404.sra -O SRR1813404
```
