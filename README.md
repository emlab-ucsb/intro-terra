---
title: "Introduction to rasters with `terra`"
author: "Jason Flower, UC Santa Barbara"
date: "2024-04-17"
output:
  html_document: 
    code_folding: show
    keep_md: true
---

<!-- README.md is generated from README.Rmd. Please edit that file -->



This workshop will provide you with an introduction to manipulating raster spatial data using the R package `terra`. `terra` and its predecessor, `raster` are widely used for spatial data manipulation in R. No prior experience of spatial data is assumed, but this short workshop will not have time to delve into some important aspects of spatial data such as projections. 

`terra`'s predecessor `raster` had many of the same functions as `terra`, and I will mention how functions have changed or been renamed which might be helpful for people migrating from using `raster`. In the words of the creator of both packages: `terra` is simpler, faster and can do more, so definitely switch to `terra` if you are still using `raster`!

*Resources*:
- The official `terra` tutorial page is https://rspatial.org/spatial/index.html  
- An excellent reference to all the functions in `terra` is https://rspatial.github.io/terra/reference/terra-package.html 

<!-- badges: start -->
<!-- badges: end -->

## Prerequisites 

You will need the `terra` package installed, which can be done by running `install.packages("terra")`. If you have problems, there are more details about installing [here](https://rspatial.github.io/terra/index.html). We can then load it


```r
library(terra)
#> terra 1.7.71
```

# Spatial data

There are basically two types of spatial data: vector and raster

## Vector data 
Can be points, lines or polygons. Useful for representing things like survey locations, rivers, and boundaries.


```{.r .fold-hide}
pts <- rbind(c(3.2,4), c(3,4.6), c(3.8,4.4), c(3.5,3.8), c(3.4,3.6), c(3.9,4.5)) |>
  vect()

lnes <- as.lines(vect(rbind(c(3,4.6), c(3.2,4), c(3.5,3.8)))) |>
  rbind(as.lines(vect(rbind(c(3.9, 4.5), c(3.8, 4.4), c(3.5,3.8), c(3.4,3.6)))))

lux <- vect(system.file("ex/lux.shp", package = "terra"))

par(mfrow = c(1,3))

plot(pts, axes = F, main = "Points")
plot(lnes, col = "blue", axes = F, main = "Lines")
plot(lux, "NAME_2", col = terrain.colors(12), las = 1, axes = F, main = "Polygons")
```

![](README_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```{.r .fold-hide}

par(mfrow = c(1,3))
```

## Raster data 
Raster data is a grid of rectangles. Each rectangle must have a value. Rasters are often used for data such as elevation, temperature and habitat maps. 


```{.r .fold-hide}
ele <- system.file("ex/elev.tif", package = "terra") |>
  rast() |>
  aggregate(fact = 2)

plot(ele, las = 1, main = "Elevation map")

ele |>
  as.polygons(aggregate = FALSE, na.rm = FALSE) |>
  lines(col = "grey40", lwd = 0.2)
```

![](README_files/figure-html/unnamed-chunk-4-1.png)<!-- -->



