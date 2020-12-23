# Brazil's Soy Crops Data

This repository used MapBiomas data to create yearly maps of Brazil's Soy cultivation. The yearly rasters are made available here. Bellow I present the tutorial of how I created them.

## Primary Data

MapBiomas 30 m resoltuion data was obtained using [Google Earth Engine](https://code.earthengine.google.com/?scriptPath=users%2Fmapbiomas%2Fuser-toolkit%3Amapbiomas-user-toolkit-download.js).[Here](https://mapbiomas.org/en/ferramentas?cama_set_language=en) is a tutorial of how to access MapBiomas in GEE. 

Data is downloaded as TIF files, which can be handled in R. 

## R Data Handling and Plotting

Install the following packages

```R
install.packages("raster")
install.packages("raster")
```

Set directory where the MapBiomas tif files are. Here I am only looking at Mato Grosso state. Since I am looking at several years I will reclass using a loop.

In the reclass function a matrix of reclassification must be given. I set rclmat as my matrix. I classify soy as "1" and other categories as "0" - MapBiomas ID for 
soybean is "39".

```R
setwd("~/Desktop/XXX")
ano<-list(2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018)

#Reclass Matrix:
m <- c(0, 38, 0,  39, 39, 1,  40, 41, 0)
rclmat <- matrix(m, ncol=3, byrow=TRUE)

for (year in ano){
   arquivo<- sprintf('mapbiomas-brazil-collection-50-matogrosso-%s- cover.tif', year)
   imported_raster<-raster(arquivo)
   reclass <- reclassify(imported_raster, rclmat)
   remove(imported_raster,arquivo)
   writeRaster(reclass,paste(sprintf('soy-%s.tif', year)))
}
```
