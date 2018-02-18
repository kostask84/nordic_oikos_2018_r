---
title: "Intro and some R setup examples"
author: "Dag Endresen, https://orcid.org/0000-0002-2352-5497"
date: "February 18, 2018"
output:
  html_document:
    keep_md: true
    toc: true
    toc_depth: 3
---

***

You are here: [R workshop](../) >> [Session 1 GBIF intro](./) >> **R settings**

![](../demo_data/NSO_2018_GBIF_NO.png "NSO 2018")

***

# Nordic Oikos 2018 - R workshop - Session 1

Scientific reuse of openly published biodiversity information: Programmatic access to and analysis of primary biodiversity information using R. Nordic Oikos 2018, pre-conference R workshop, 18<sup>th</sup> and 19<sup>th</sup> February 2018. Further information [here](http://www.gbif.no/events/2018/Nordic-Oikos-2018-R-workshop.html).


**Session 1** gives an introduction to [GBIF](https://www.gbif.org/), use of [GBIF data](https://www.gbif.org/resource/search?contentType=dataUse) and the [GBIF API](https://www.gbif.org/developer/summary).

Notice that most of the R-commands below are commented out (with a hash #). To execute selected lines on your computer, you need to uncomment (remove the hash #) first.

***


```r
## Notice that eval=FALSE will exclude execution of this chunk in knitr, but enable manual execution in RStudio.
## Setting the working directory, here: to the same directory as the RMD-script. Primarily useful "outside" of RMarkup chuks.
## RMarkup (Rmd) seems to always change current working directory to the same as the Rmd-script.
require(rstudioapi)
setwd(dirname(rstudioapi::getActiveDocumentContext()$path))
getwd() ## will display the working directory
```

```
[1] "/Users/dag/workspace/gitHub/nordic_oikos_2018_r/s1_gbif_intro"
```

### Create a folder for demo data

```r
## Note however, that RMarkdown with knitr works best with all files in the same directory
#dir.create(file.path("./demo_data")) ## create a folder for demo files (inside working directory)
#dir.create(file.path("../demo_data")) ## create a folder for demo files (next to working directory)
#dir.create(file.path("..", "demo_data")) # will work better accross OS? on Windows?
```

### Some useful R-packages to install, that we will use during the workshop

```r
## It is possible to install R-packages from the script
install.packages("rmarkdown") # R Markdown
## rOpenSci packages
install.packages("rgbif") # species occurrence data from GBIF
install.packages("mapr", dependencies = TRUE) # rOpenSci mapping tools
install.packages("spocc", dependencies = TRUE) # species occurrence, more than GBIF
## Some mapping packages
install.packages("maps") # mapping tools
install.packages("mapproj") # map projection tools
install.packages("rgdal") # r gdal tools
install.packages(mapdata) # map data
## Dismo and Raster by R Hijmans
install.packages("dismo") # species occurrence data from GBIF with dismo
install.packages("raster") # raster data tools
##
install.packages("plotly")
install.packages("RColorBrewer")
##
install.packages("tidyverse") ## tidyverse
install.packages("readxl") ## Read Excel, from tidyverse (without external Java dependencies)
##
##Sys.setenv(JAVA_HOME="PATH-TO-JAVA_HOME")
#Sys.setenv(JAVA_HOME='/Library/Java/Home') ## or use '/usr/libexec/java_home -d 64'
#Sys.setenv(JAVA_HOME='/Library/Java/JavaVirtualMachines/jdk1.7.0_45.jdk/Contents/Home')
### macOS High Sierra Version 10.13.3
install.packages("rJava")
install.packages("xlsx")
##
```

### Load R-packages

```r
# Use functions library() or require() to load the R-packages you need

#require(rgbif) # r-package for accessing GBIF
#library('mapr') # rOpenSci r-package for mapping (occurrence data)
#library('spocc') # rOpenSci r-package with more biodiversity data sources than GBIF
#require(maps)
#require(mapproj)
#require(mapdata)
#require(maptools) # mapping tools for spatial objects
#require(rgdal) # provides the Geospatial Data Abstraction Library
#require(raster) # spatial raster data management
#library(gapminder)
#library(tidyverse) ## includes dplyr etc for handling data, select(), filter()
#library('plyr') ##
#library('RColorBrewer')
```

### Clear workspace (be careful)

```r
# Large arrays and dataframes can consume huge parts of your working memory,
# and you probably want to remove some of them from memory when they are not needed anymore.

#rm(list = ls()) # Be careful, this line cleans the entire R workspace
#rm(sp) # Probably better to remove large arrays and dataframes individually
```


***

[R Markdown](http://rmarkdown.rstudio.com/) is a convinient format (R Notebooks) for writing R-scripts, introduced in 2014. R Markdown integrates very well in RStudio (and can also be used form the command line). [Session 2](../s2_r_intro) will provide an introduction to R-scripting including R Markdown.



***
***
***
***
***
***
**NOTES**
***
***
***


```r
## BOUNDING BOX
##bb <- c(lowerLon, lowerLat, higherLon, higherLat) # bounding box
bb <- c(10.2,63.3,10.6,63.5) # Trondheim
bb <- c(5.25, 60.3, 5.4, 60.4) # Bergen
bb <- c(18.7, 69.6, 19.2, 69.8) # Tromsoe
bb <- c(10.6, 59.9, 10.9, 60.0) # Oslo
bb <- c(4.5, 54.9, 31.0, 71.0) # Scandinavia = lon(4.5, 31.0), lon(55, 71)
##
bbox(norway_mask) ## use bbox(spatialObj) to find the bounding box of any spatial object
```

Norway:
         min       max
x  4.6280575 31.078054
y 58.0702763 71.113054


```r
## Print text and variables
message(sprintf("Current working dir: %s\n", getwd()))
paste("Today is", date())
```


```r
sprintf(5.55555, fmt = '%#.3f') ## 5.556
sprintf(5.55555, fmt = '%#.3g') ## 5.56
```


```r
library('plyr') ## r-pkg plyr for data handling
spp_df <- ldply(spp) ## ldply - split list, apply function, return dataframe (here list to df)
```


```r
crs(r) <- "+proj=longlat +datum=WGS84 +no_defs +ellps=WGS84 +towgs84=0,0,0"
crs(r) <- CRS('+init=EPSG:4326') ## same result, but using EPSG is shorter to write, and easier
```

List to dataframe

```r
#spp <- occ_search(taxonKey=keys, limit=100, return='data', country='NO', hasCoordinate=TRUE)
library('plyr') ## r-pkg plyr for 
spp_df <- ldply(spp) ## ldply - split list, apply function, return dataframe (here list to df)
#spp_m <- spp_df[c("name", "decimalLongitude","decimalLatitude", "basisOfRecord", "year", "municipality")]
#map_leaflet(spp_m, "decimalLongitude", "decimalLatitude", size=3, color=cols)
```

Read.xlsx depend on JAVA which might be an obstacle

```r
##Sys.setenv(JAVA_HOME="PATH-TO-JAVA_HOME")
#Sys.setenv(JAVA_HOME='/Library/Java/Home') ## or use '/usr/libexec/java_home -d 64'
#Sys.setenv(JAVA_HOME='/Library/Java/JavaVirtualMachines/jdk1.7.0_45.jdk/Contents/Home')
library(xlsx) ## first row contains variable names
spp_dq <- read.xlsx("./demo_data/spp_dq.xlsx", 1) ## manipulated occurrence data, introduced errors
#spp_da <- read.xlsx("./demo_data/spp_dq.xlsx", sheetName = "spp_dq") ## alt: choose named worksheet
head(spp_da, n=5)
```

***

Diverse color palettes

```r
library(RColorBrewer)
#display.brewer.all()
display.brewer.pal(n=9, name='Set1')
```

![colorBrewer Set1](./demo_data/display-brewer-pal_Set1.png "colorBrewer Set1")


Read more about colors at the [https://www.r-bloggers.com/palettes-in-r/](R-bloggers story about color palettes in R)

***
