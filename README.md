# Brazil's Soy Crops Data

This repository used MapBiomas data to generate yearly data of Mato Grosso's Soy cultivation. Bellow I present the tutorial of how I extract such information.

## Primary Data

MapBiomas 30 m resoltuion data was obtained using [Google Earth Engine](https://code.earthengine.google.com/?scriptPath=users%2Fmapbiomas%2Fuser-toolkit%3Amapbiomas-user-toolkit-download.js).[Here](https://mapbiomas.org/en/ferramentas?cama_set_language=en) is a tutorial of how to access MapBiomas in GEE. 

Data is downloaded as TIF files, which can be handled in R. 

## R Data Handling

Install the following packages

```R
install.packages(c("raster","stars","sf"))
```

Set directory where the MapBiomas tif files are. Here I am only looking at Mato Grosso state. Since I am looking at several years I will reclass using a loop.

In the reclass function a matrix of reclassification must be given. I set rclmat as my matrix. I classify soy as "1" and other categories as "0" - MapBiomas ID for 
soybean is "39".

```R
library(raster)
library(stars)
library(sf)
setwd("~/Desktop/XXX")
ano<-list(2006,2007,2008,2009,2010,2011,2012,2013,2014)

#Reclass Matrix:
m <- c(0, 38, 0,  38, 39, 1,  39, 41, 0)
rclmat <- matrix(m, ncol=3, byrow=TRUE)

for (year in ano){
   arquivo<- sprintf('mapbiomas-brazil-collection-50-matogrosso-%s- cover.tif', year)
   imported_raster<-raster(arquivo)
   reclass <- reclassify(imported_raster, rclmat)
   remove(imported_raster,arquivo)
   writeRaster(reclass,paste(sprintf('soy-%s.tif', year)))
}
```

Now I will use the stars function to stack and transform the raster data in a sf format:

```R
soy_cover_mt<-stack(`soy_cover_mt-2006`,`soy_cover_mt-2007`,`soy_cover_mt-2008`,`soy_cover_mt-2009`,`soy_cover_mt-2010`,`soy_cover_mt-2011`,`soy_cover_mt2012`,`soy_cover_mt-2013`,`soy_cover_mt-2014`)
writeRaster(soy_cover_mt,filename="soy_cover-mt.tif",format="GTiff")
camadast<-read_stars("soy_cover-mt.tif")
soy_cover_frame_mt<-st_as_sf(camadast,as_points=TRUE)
save(soy_cover_frame_mt,file="soy_cover_frame_mt_1km.RData")
remove(`soy_cover_mt-2006`,`soy_cover_mt-2007`,`soy_cover_mt-2008`,`soy_cover_mt-2009`,`soy_cover_mt-2010`,`soy_cover_mt-2011`,`soy_cover_mt-2012`,`soy_cover_mt-2013`,`soy_cover_mt-2014`)
```

