---
published: true
title: "MultiDataSet"
layout: post
date: 2017-01-17 08:00
projects: true
hidden: true # don't count this post in blog pagination
description: "MultiDataSet is designed for integrating multi omics data sets and ResultSet is a container for omics results."
category: project
author: carleshf
externalLink: false
image: https://carleshf.github.io/assets/portraits/multiple_datasets_coordination.png
headerImage: true
---


## Summary

Reduction in the cost of genomic assays has generated large amounts of biomedical-related data. As a result, current studies perform multiple experiments in the same subjects. MultiDataSet is a new R class based on Bioconductor standards and designed to encapsulate multiple data sets. MultiDataSet deals with the usual difficulties of managing multiple and non-complete data sets while offering a simple and general way of sub-setting features and selecting samples. We illustrate the use of MultiDataSet in three common situations: 1) performing integration analysis with third party packages; 2) creating new methods and functions for omic data integration; 3) encapsulating new unimplemented data from any biological experiment.

This package contains base classes for `MEAL` and `omicRexposome` packages.

## Results

This project published an R package in Bioconductor:

* Bioconductor: [link](https://bioconductor.org/packages/release/bioc/html/MultiDataSet.html)
* GitHub: [link](https://github.com/isglobal-brge/MultiDataSet)

And with a publication in **BMC Bioinformatics** [(Open Access)](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-016-1455-1):

> MultiDataSet: an R package for encapsulating multiple data sets with application to omic data integration

*Carles Hernandez-Ferrer*, Carlos Ruiz-Arenas, Alba Beltran-Gomila and Juan R. Gonzalez / BMC Bioinformatics 2017 / doi:10.1186/s12859-016-1455-1
