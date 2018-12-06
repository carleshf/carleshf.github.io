---
published: true
layout: post
category: blog
title: Comparing 'user' Internet connection from some Catalan research centers
author: Carles Hernandez-Ferrer
date: '2017-07-31'
---

Using the same technique seen in the old post "*Comparing ping time between connections*" I asked some colleges to run the following command in their research centers.

```bash
ping www.google.com -c 200 > ping_google.txt
```

So, I load the multiple `ping-files` to create a `data.frame` with the `icmp_seq` number, the time spend per ping and the institution where the ping was promoted.

```R
ping <- lapply( files, function( file ) {
    dta  <- read.delim( file[ 2 ], nrows = 200, skip = 1, header = FALSE, 
        sep = " ", stringsAsFactors = FALSE )[ , c( 6, 8 ) ]
    colnames( dta ) <- c( "icmp_seq", "time" )
    dta$icmp_seq <- as.numeric( gsub( "icmp_seq=", "", dta$icmp_seq ) )
    dta$time <- as.numeric( gsub( "time=", "", dta$time ) )
    dta$institution <- file[ 1 ]
    dta
})
ping <- do.call( rbind, ping )
ping[ 1:3, ]
```
```
##   icmp_seq time institution
## 1        1 10.2    ISGlobal
## 2        2 10.1    ISGlobal
## 3        3 10.2    ISGlobal
```

The next steps is to create a line-plot with the time used to ping *Google* in each institution.

```R
library( ggplot2 )

colors <- c("#FFA500", "#20B2AA", "#9ACD32", "#BA55D3", "#4169E1", "#F08080")
names( colors ) <- c("ISGlobal", "IRB", "IBB", "IDIBELL",  "BSC-CNS", "VHIR")

g_t <- ggplot( ping, aes( x = icmp_seq, y = time, group = institution ) ) +
    theme_minimal() +
    geom_line( aes( color = institution ) ) +
    ylab( "time (ms)" ) +
    theme( legend.position = "top" ) +
    scale_color_manual( values = colors )
```

I saved the plot in `g_t` in order to create a panel in one of the last steps.

I already asked my colleagues to perform a speed test so we can know the speed to download an upload files. I saves this information into a `csv` file.

```R
science <- read.delim( "data/ping_data/up_down.csv", sep = "," )[ , -3]
```

The `csv` file, loaded as `science`, contains the location of each one of the institutions were we work:

```R
library( ggmap )

qmap( "Barcelona", maptype = "toner-lite", zoom = 12, source = "stamen" ) +
    geom_point( data = science, aes( x = lat, y = lon ), 
                col = "orange", alpha = 0.4, size = 7 )
```

![Map with the location of the Catalan research centers]({{baseurl}}/assets/catalan-research-centers-map.png)


An the following creates a leaf-plot with both download and upload speed. The `grobs` objects from `ggplot2` are save in variables to create a panel joint with the line-plot created before.

```R
mm <- max( c( science$download, science$upload ) )

g_m <- ggplot( data = science, aes( x = 1, y = institution ) ) +
    geom_text( aes( label = institution ) ) +
    ggtitle( "" ) + theme_void()

g_l <- ggplot( data = science, aes(x = institution, y = download ) ) +
    geom_bar( stat = "identity", aes( fill = institution ) ) +
    scale_fill_manual( values = colors ) +
    ggtitle( "Download Speed" ) +
    theme_linedraw() +
    theme(
        axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        axis.text.y = element_blank(),
        axis.ticks.y = element_blank(),
        plot.margin = grid::unit( c( 1, -1, 1, 0 ), "mm" ),
        plot.title = element_text(hjust = 0.5),
        legend.position = "none"
    ) +
    scale_y_reverse( limits = c( mm , 0 ) ) +
    coord_flip()

g_r <- ggplot( data = science, aes( x = institution, y = upload ) ) +
    xlab( NULL ) +
    geom_bar( stat = "identity", aes( fill = institution ) ) +
    scale_fill_manual( values = colors ) + 
    ggtitle( "Upload Speed" ) +
    theme_linedraw() +
    theme(
        axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        axis.text.y = element_blank(),
        axis.ticks.y = element_blank(),
        plot.margin = grid::unit( c( 1, 0, 1, -1 ), "mm" ),
        plot.title = element_text(hjust = 0.5),
        legend.position = "none"
    ) +
    scale_y_continuous( limits = c( 0 , mm ) ) +
    coord_flip()

g_l <- ggplot_gtable( ggplot_build( g_l ) )
g_r <- ggplot_gtable( ggplot_build( g_r ) )
g_m <- ggplot_gtable( ggplot_build( g_m ) )
```

The result follows:

```R
library( gridExtra )

g_b2 <- ggplot_gtable( ggplot_build( g_t ) )

grid.arrange( g_b2, g_l, g_m, g_r, 
    layout_matrix = rbind( c( 1, 1, 1 ),
                            c( 2, 3, 4 ) ),
    widths = c( 4/9, 1/9, 4/9 ) )
```
![Three charts indicating the speed and capacity of the research centers connection]({{baseurl}}/assets/catalan-research-centers-charts.png)
