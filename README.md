# Brazil's Soy Crops Data

This repository used MapBiomas data to create yearly maps of Brazil's Soy cultivation. The yearly rasters are made available here. Bellow I present the tutorial of how I created them.

## Primary Data

MapBiomas 30 m resoltuion data was obtained using [Google Earth Engine](https://code.earthengine.google.com/?scriptPath=users%2Fmapbiomas%2Fuser-toolkit%3Amapbiomas-user-toolkit-download.js).[Here](https://mapbiomas.org/en/ferramentas?cama_set_language=en) is a tutorial of how to access MapBiomas in GEE. 

Data is downloaded as TIF files, which can be handled in R. 

## R Data Handling

Install the following packages

```R
install.packages("raster")
```

