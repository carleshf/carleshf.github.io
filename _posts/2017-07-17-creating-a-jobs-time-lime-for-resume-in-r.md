---
published: true
layout: post
title: Creating a jobs time-lime for resume in R
---

Let's say we define a `data.frame` with the jobs I've got from 2008 to 2017:


```R
jobs <- data.frame(
  employer    = c( "GICO", "TES", "UAB", "IFAE", "ISGlobal" ),
  year_start  = as.Date( c( "2008-07-01", "2009-11-01", "2010-09-01", "2011-07-01", "2013-09-01" ) ),
  year_end    = as.Date( c( "2009-10-31", "2010-07-31", "2011-06-30", "2012-09-30", "2017-08-31" ) ),
  id          = 1,
  stringsAsFactors = FALSE
)
```

The content of the `data.frame` is easy understandable:

  * The `employer` shows the nanme of the compaty/institution who employed me.
  * The `year_start` indicates the date when I started to work for a given emplyer.
  * The `year_end` indicates the date when I stoped working for a given emplyer.

The dificult to understand is the `id`. It indicates the *track* where the `employer` whill be shown. I want to see all employers in the same *track*, so `id` has the same value for all of the entries in the `data.frame`.

Then we start working with `ggplot`:

```R
library(ggplot2)
```

We will take advantage of the `geom_segment` to create the *time line*:

```R
p <- ggplot( jobs, aes( colour = employer) ) +
  geom_segment(
    aes( x = year_start, xend = year_end, y = employer, yend = employer ), size = 7 
  ) 
p
```
![Fragmented time line]({{baseurl}}/assets/time-line-in-r-1.png)

Next step sit to glat the segments to a single line. To this end we use the `id`:

```R
p <- ggplot( jobs, aes( colour = employer) ) +
  geom_segment(
    aes( x = year_start, xend = year_end, y = id, yend = id ), size = 7 
  ) 
p
```
![Time line in order]({{baseurl}}/assets/time-line-in-r-2.png)

Now, we clean the environemnt to get a clear view of the *time line*.

```R
p <- p + theme_minimal() + xlab( "" ) + ylab( "" ) +
  theme(
    legend.position = "top",
    axis.text.x  = element_text(angle = 90, hjust = 1),
    axis.title.y = element_blank(),
    axis.text.y  = element_blank(),
    axis.ticks.y = element_blank()
  )
p
```
![Final time line - muliple colors]({{baseurl}}/assets/time-line-in-r-3.png)

Optionally we can update the colors:

```R
pal <- colorRampPalette(c("LightCyan", "DarkCyan"))
colors <- pal(length(unique(jobs$employer)))
names(colors) <- unique(jobs$employer)

p + scale_colour_manual(values=colors)
```
![Final time line - monochromatic]({{baseurl}}/assets/time-line-in-r-4.png)
