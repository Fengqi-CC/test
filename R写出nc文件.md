rm(list=ls())
library(raster)
library(rasterVis)
library(RColorBrewer)
library(ncdf4)
library(rgdal)
library(ggplot2)
library(viridis)  
library(ggthemes) 
library(sf)
library(maptools)

vals1 = seq(109.78161,124.96637,(124.96637-109.78161)/340)
vals2 = seq(28.50404,43.13213,(43.13213-28.50404)/410)

vals1 = seq(108.88161,124.16637,(124.16637-108.88161)/340)
vals2 = seq(28.60404,43.23213,(43.23213-28.60404)/410)

longitude <- ncdim_def( name = 'longitude', units = 'degrees_east', vals1[1:340] )
latitude <- ncdim_def( name = 'latitude', units = 'degrees_north', vals2[1:410])
t <- ncdim_def( name = 'time', units = 'years', vals = 0:18)

#dim = ncdim_def('Tmax_d', 'K', Tmax_d_d, unlim=FALSE, create_dimvar=TRUE, calendar=NA, longname='Tmax_daytime')
vars = ncvar_def('Tmax_d1', 'K', dim = list(longitude,latitude,t), NA, longname='T2max_daytime', prec="float", shuffle=FALSE,
       compression=NA, chunksizes=NA, verbose=FALSE )
ncnew = nc_create('K:/Surfex_Meteo_factors/Test/def_test3.nc', vars, force_v4=FALSE, verbose=FALSE)
ncvar_put(nc = ncnew, varid=vars, vals=Tmax_d_d)
ncatt_put( nc = ncnew, varid = 0, attname = 'description', attval = 'sst data in NINO3 area during 2009')
nc_close(ncnew)

#打开检查
nc <- nc_open('K:/Surfex_Meteo_factors/Test/def_test1.nc')
uu <- ncvar_get(nc, varid=vars, start=NA, count=NA, verbose=FALSE, signedbyte=TRUE, collapse_degen=TRUE)

