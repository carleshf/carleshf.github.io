---
published: true
title: Boston's Temperature Chart
category: blog
layout: post
author: carleshf
tags:
  - R
  - ggplot2
---


At the end of January I will be moving to Boston. I will start my post-doc at the [Boston Children's Hospital](www.childrenshospital.org). So... I started looking to weather and temperature conditions. I used [Weather Underground](https://www.wunderground.com) to download a weatehr tamble for each month in 2016 and 2017.

The aim is to create a plot with everyday minimum and maximum temperature along all 2017. Also a heat-map indicating the weather condition of each day of the year.

```R
data_files <- list.files( data_path, full.names = TRUE )
basename( data_files )
```

The header of each one of tha downloaded tables corresponds to:

```R
header <- c( "Day", "tmp_high", "tmp_avg", "tmp_low", "dewp_high", "dewp_avg",
    "dewp_low", "hum_high", "hum_avg", "hum_low", "seap_high", "seap_avg", 
    "seap_low", "vis_high", "vis_avg", "vis_low", "wind_high_1", "wind_avg", 
    "wind_high_2", "precip", "events" )
```

I also translated the names of the months:

```R
month_names <- c( "jan" = "January", "feb" = "February", "mar" = "March", 
    "apr" = "April", "may" = "May", "jun" = "June", "jul" = "July", 
    "aug" = "August", "sep" = "September", "oct" = "October", 
    "nov" = "November", "dec" = "December " )
```

Then I read each table:

```R
data <- lapply( data_files, read.delim, skip = 1, sep = "\t", col.names = header )
names( data ) <- sapply( data_files, basename )
```

Once the data is lodead, I add to each table the name of the file, the month and the year.

```R
data <- lapply( names( data ), function( nm ) { 
  data[[ nm ]]$File <- nm
  data[[ nm ]]$Month <- month_names[ strsplit( nm, "_" )[[1]][3] ]
  data[[ nm ]]$Year <- strsplit( nm, "_" )[[1]][1]
  data[[ nm ]] 
} )
names( data ) <- sapply( data_files, basename )
```

Once the tables are properly formated I subest only those used in the plots:

```R
sel_2017 <- names( data )[ substr( names( data ), start = 1, stop = 4) == "2017" ]
```

Using `reshape2` package we *melt* the list of tables to be used with `ggplot2`:

```R
library( reshape2 )

vars <- c( "Day", "Month", "Year" )
data_melt <- melt( lapply( sel_2017, function( nm ) data[[ nm ]][ , c( vars, "tmp_high", "tmp_low" ) ] ), id.vars = vars )
```

It is important to be sure `Month` is a `factor` and that this `factor` is properly ordered:

```R
data_melt$Month <- factor( data_melt$Month, levels = month_names )
```

Finally we plot the temperature chart:

```R
library( ggplot2 )

ggplot( data_melt, aes( x = Day, y = value, fill = variable ) ) + 
  geom_bar( stat = "identity", width = 0.5, position = "dodge" ) +
  facet_grid( Month ~ ., scales = "free" ) +
  scale_fill_manual( values = c( "LightCoral","RoyalBlue" ) ) +
  theme_bw() +  scale_x_continuous( breaks = 1:31 ) +
  ylab( "" ) + ggtitle( "Boston Temperature 2017" ) +
  theme( legend.position = "top" )
```

![Boston's Temperature 2017]({{baseurl}}/assets/boston-weather-temperature.png)

For the weather conditions heat-map I start creating the colors codification:

```R
color_events = c( "*" = "White",
  "Fog" = "Azure",
  "Fog , Rain" = "#CEE7FF",
  "Fog , Rain , Snow" = "LightSkyBlue",
  "Fog , Rain , Thunderstorm" = "IndianRed",
  "Fog , Snow" = "MediumPurple",
  "Rain" = "RoyalBlue",
  "Rain , Snow" = "#4B0082",
  "Rain , Thunderstorm" = "DarkSlateBlue",
  "Rain , Snow , Thunderstorm" = "Black",
  "Snow" = "Purple",
  "Thunderstorm"  = "Maroon"
)
```

Then I create the table:

```R
data_melt <- melt( lapply( sel_2017, function( nm ) {
    data[[ nm ]][ , c( vars, "events" ) ]
}), id.vars = c( vars, "events" ) )
data_melt$Month <- factor( data_melt$Month, levels = rev( month_names ) )
data_melt$events <- gsub( "^\\s+|\\s+$", "", data_melt$events )
data_melt$events[ data_melt$events == "" ] <- "*"
```

And the last step is to create the heat-map:

```R
ggplot( data_melt, aes( x = Day, y = Month, fill = events ) ) + geom_tile() +
  theme_bw() +  scale_x_continuous( breaks = 1:31 ) +
  scale_fill_manual(values = color_events ) +
  ylab( "" ) + ggtitle( "Boston Weather Events 2017" ) +
  scale_alpha_manual( values = 0.3 )
```

![Boston's Weather Condition 2017]({{baseurl}}/assets/boston-weather-conditions.png)
