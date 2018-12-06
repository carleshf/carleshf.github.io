---
published: true
title: "CTDquerier"
layout: post
date: 2014-09-21 22:10
projects: true
hidden: true # don't count this post in blog pagination
description: "R package to query the Comparative Toxicogenomics Database from R session and use its data for downstream or enrichment analysis."
category: project
author: carleshf
externalLink: false
---

## Summary

The CTDquerier R/Bioconductor package allows users to query CTD by gene, by chemical and by disease using single or multiple terms. The terms are validated against the CTD vocabulary used to download the multiple results from cTD as TSV files. The TSV files are read as DataFrames and once encapsulated in an S4 object of class CTDdata. Three methods are provided for CTDdata objects: (i) get_terms retrieves the terms that are validated in CTD vocabulary files; (ii) get_table fetches data from CTD as an object of class DataFrame; and (iii) enrich performs a Fisher’s exact test between two CTDdata objects.

## Results

This project published an R package in Biocondcutor:

* Bioconductor: [link](https://bioconductor.org/packages/release/bioc/html/CTDquerier.html)
* GitHub: [link](https://github.com/isglobal-brge/CTDquerier)

And with a publication in **Bioinformatics**:

> CTDquerier: A Bioconductor R package for Comparative Toxicogenomics DatabaseTM data extraction, visualization and enrichment of environmental and toxicological studies.

*Carles Hernandez-Ferrer* and Juan R. Gonzalez
Bioinformatics 2018 / doi:10.1093/bioinformatics/bty326 /

[(Open Access)](https://academic.oup.com/bioinformatics/advance-article/doi/10.1093/bioinformatics/bty326/4983065)