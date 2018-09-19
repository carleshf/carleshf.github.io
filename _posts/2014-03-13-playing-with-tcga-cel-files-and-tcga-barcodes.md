---
published: true
layout: post
title: Playing with TCGA .CEL files and TCGA Barcodes
author: Carles Hernandez-Ferrer
date: '2014-03-13'
slug: playing-with-tcga-cel-files-and-tcga-barcodes
categories:
  - R
  - bioinformatics
tags:
  - TCGA
---

Today I want a file relating the names of the .CEL files from TCGA, the barcodes for this samples and the definition of the sample type in the three available forms (numeric, short and description). An example, the following:

```
filename        barcode sampletype_numeric      sampletype_short        sampletype_desc
TCGA_666_A01_0070X01.CEL   TCGA-ZZ-A6AW-01A-01A-X00D-AB    01      TP      Primary solid Tumor
TCGA_666_A02_0070X01.CEL   TCGA-ZZ-A6AW-10A-01A-X00G-AB    10      NB      Blood Derived Normal
TCGA_666_A03_0070X01.CEL   TCGA-ZZ-A6AW-01A-01A-X00D-AB    01      TP      Primary solid Tumor
```

In order to provide a reusable way to create this file I wrote the following function in R:

```R
tcga_barc_tbl <- function(typefile, mapfile, outfile="TABLE_TCGA.TSV") {
    nnD <- read.delim(mapfile, header=TRUE)
    stD <- read.delim(typefile, header=TRUE, sep=",")

    x <- lapply(as.character(nnD[ , "barcode.s."]), function(x){ 
            wd <- strsplit(strsplit(x, ",")[[1]], "-")[[1]][4]
            substr(wd, 1, nchar(wd)-1)
        }
    )

    df <- data.frame(filename=nnD[ , "filename"], barcode=nnD[ , "barcode.s."], 
        sampletype_numeric=unlist(x), 
        sampletype_short=stD[as.numeric(unlist(x)), "Short.Letter.Code"],
        sampletype_desc=stD[as.numeric(unlist(x)), "Definition"])

    if (is.na(outfile)) {
        return(df)
    } else {
        write.table(df, file=outfile, quote=FALSE, sep="t", row.names=FALSE)
    }
}
```

The function tgca_barc_tbl needs the FILE_SAMPLE_MAP.txt and the sampleType.csv. The first one comes in the .tar.gz file when downloading from TGCA - Data Matrix. The second one can be generated at codeTableReport

Feel free to use it at your own.
