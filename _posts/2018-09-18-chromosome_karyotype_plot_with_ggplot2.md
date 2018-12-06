---
published: true
layout: post
category: blog
title: practical ggplot2 - Creating a chromosome karyotype plot
---

The first step is to get the size of each chromosome and the location of the centromeres. I download the tables "chromInfo" for chromosome size and "gap" for centromeres annotation from the UCSC (http://genome.ucsc.edu/cgi-bin/hgTables).

The table "chromInfo" contains the size of each chromosome

```
#chrom	size	fileName
chr1	249250621	/gbdb/hg19/hg19.2bit
chr2	243199373	/gbdb/hg19/hg19.2bit
chr3	198022430	/gbdb/hg19/hg19.2bit
chr4	191154276	/gbdb/hg19/hg19.2bit
chr5	180915260	/gbdb/hg19/hg19.2bit
[...]
```

The table "gap" contains the location of different elements, including the centromeres:

```
#bin	chrom	chromStart	chromEnd	ix	n	size	type	bridge
0	chr1	124535434	142535434	1271	N	18000000	heterochromatin	no
23	chr1	121535434	124535434	1270	N	3000000	centromere	no
76	chr1	3845268	3995268	47	N	150000	contig	no
85	chr1	13219912	13319912	154	N	100000	contig	no
```

With the tables saved as files ( "chromInfo" as `chrm_size.tsv` and "gap" as `gap_table.tsv` ) we load and preprocess them:

```R
# Load chromosomes size and get chromosomes from 1 to 22
chrms <- read.delim( "chrm_size.tsv", sep = "\t" )
chrms <- chrms[ gsub( "chr", "", chrms$X.chrom ) %in% as.character( seq( 22 ) ), c( "X.chrom", "size" ) ]
chrms <- data.frame(
	chrom = as.numeric( gsub( "chr", "", chrms$X.chrom ) ),
	chromStart = 0,
	chromEnd = chrms$size
)

# Load centromeres location
centros <- read.delim( "gap_table.tsv", sep = "\t" )
centros <- centros[ centros$type == "centromere", ]
centros <- centros[ gsub( "chr", "", centros$chrom ) %in% as.character( seq( 22 ) ), c( "chrom", "chromStart", "chromEnd" ) ]
centros$chrom <- as.numeric( gsub( "chr", "", centros$chrom ) )
```

With the tables loaded we can use `ggplot2` and `geom_segments` to create a box per each chromosome and then to create a smaller box for the centromer:

```R
library( ggplot2 )
ggplot() +
	geom_segment( data = chrms,
				aes( y = 0, yend = 0, x = chromStart, xend = chromEnd ),
				lineend = "round", color = "Gainsboro", size = 5 ) +
	geom_segment( data = centros, 
				aes( y = 0, yend = 0, x = chromStart, xend = chromEnd ),
				lineend = "round", color = "DimGray", size = 5 ) +
	scale_x_continuous( breaks = seq( 0, 2.5e8, 50e6 ), labels = c( 1, seq( 50, 250, 50  ) ) ) +
	theme_minimal() +
	theme(
	legend.position = "top",
	axis.text.y = element_blank(),
	axis.ticks.y = element_blank(),
	panel.grid.major = element_blank(), 
	panel.grid.minor = element_blank()
	) + 
	facet_grid( chrom ~ ., switch = "y" ) +
	ylab( "" ) + xlab( "Genomic positions (Mb)" )
```

The previous code shows a karyotype map:

![Karyotype plot with ggplot2]({{baseurl}}/assets/ggplot2_karyotype.png)

Over this diagram other information can be added with extra `geom_segments`.
