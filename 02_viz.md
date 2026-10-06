Visualization 2
================
Sameera
2026-10-06

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(p8105.datasets)
library(patchwork)
data("weather_df")
```

start w scatterplot

``` r
weather_df |> 
  ggplot(aes(x = tmax, y= tmin, colour = name)) +
  geom_point() +
  labs(
    title = "Temperature (Max vs Min)",
    x = "Max Temperature (C)",
    y = "Min Temperature (C)",
    color = "Location",
    caption = "Data from NOAA for three weather stations"
  ) +
  
  scale_x_continuous(
    breaks = c(-10, 0, 15),
    labels = c("-10 C", "0", "Fifteen")
  ) +
  scale_y_continuous(
    trans = "sqrt",
    position = "right"
  )
```

    ## Warning in transformation$transform(x): NaNs produced

    ## Warning in scale_y_continuous(trans = "sqrt", position = "right"): sqrt
    ## transformation introduced infinite values.

    ## Warning: Removed 520 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

Color

``` r
weather_df |> 
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() +
  scale_colour_hue(h = c(500, 950))
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
weather_df |> 
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() +
  viridis::scale_colour_viridis(
    name = "Location",
    discrete = TRUE
  )
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## Themes

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax, colour = name)) +
  geom_point() +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  ) +
  theme_classic()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
  theme(legend.position = "bottom")
```

    ## <theme> List of 1
    ##  $ legend.position: chr "bottom"
    ##  @ complete: logi FALSE
    ##  @ validate: logi TRUE

update tmax vs date plot

``` r
weather_df |> 
  ggplot(aes(x = date, y = tmax, colour = name)) +
  geom_point() + 
  geom_smooth(se = FALSE) +
  labs(
    title = "Seasonal trends in Max Temp",
    x = "Date",
    y = "Max Temp",
    caption = "Max daily temp in three weather stations in 2021 and 2020",
    color = "Location"
  ) +
  viridis::scale_color_viridis(
    discrete = TRUE
  ) +
  theme_minimal() +
  theme(legend.position = "bottom")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

## Two more useful plot things

``` r
central_park_df =
  weather_df |> 
  filter(name == "CentralPark_NY")

molokai_df =
  weather_df |> 
  filter(name == "Molokai_HI")

ggplot(molokai_df, aes(x = date, y = tmax, color = name)) +
  geom_point() +
  geom_line(data = central_park_df)
```

    ## Warning: Removed 1 row containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

## Multiple panels w diff plot types

``` r
ggp_tmax_tmin =
  weather_df |> 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point() +
  theme(legend.position = "none")

ggp_prcp_density = 
  weather_df |> 
  filter(prcp > 0) |> 
  ggplot(aes(x = prcp, fill = name)) +
  geom_density(alpha = .5) +
  theme(legend.position = "none")

ggp_seasonal =
  weather_df |> 
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point() +
  theme(legend.position = "bottom")

(ggp_tmax_tmin + ggp_prcp_density) / ggp_seasonal
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).
    ## Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->
