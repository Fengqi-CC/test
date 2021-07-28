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

#load('K:/2 Irrigation+urbanization/data_process/T2m_sig/FLux+T2M+noirr/2000-2018T2m_6pro_def.RData')
#load('K:/2 Irrigation+urbanization/data_process/T2m_sig/FLux+T2M+noirr/2000-2018T2m_6pro_Ub.RData')
#load('K:/2 Irrigation+urbanization/data_process/T2m_sig/FLux+T2M+noirr/2000-2018Flux_8pro_def.RData')
#load('K:/2 Irrigation+urbanization/data_process/T2m_sig/FLux+T2M+noirr/2000-2018Flux_8pro_Ub.RData')


load('K:/2 Irrigation+urbanization/data_process/T2m_sig/FLux+T2M+irr/2000-2018Flux_8pro_def.RData')
load('K:/2 Irrigation+urbanization/data_process/T2m_sig/FLux+T2M+irr/2000-2018Flux_8pro_Real.RData')
load('K:/2 Irrigation+urbanization/data_process/T2m_sig/FLux+T2M+irr/2000-2018Flux_8pro_Ub.RData')
load('K:/2 Irrigation+urbanization/data_process/T2m_sig/FLux+T2M+irr/2000-2018T2m_6pro_def.RData')
load('K:/2 Irrigation+urbanization/data_process/T2m_sig/FLux+T2M+irr/2000-2018T2m_6pro_Real.RData')
load('K:/2 Irrigation+urbanization/data_process/T2m_sig/FLux+T2M+irr/2000-2018T2m_6pro_Ub.RData')


#nc文件的批量写出
#vals1 = seq(109.78161,124.96637,(124.96637-109.78161)/340)
#vals2 = seq(28.50404,43.13213,(43.13213-28.50404)/410)

names = c("GF_d_def", "GF_d_real", "GF_d_ub", "GF_n_def", "GF_n_real", "GF_n_ub" ,"H_d_def","H_d_real",
           "H_d_ub", "H_n_def", "H_n_real", "H_n_ub", "LE_d_def", "LE_d_real", "LE_d_ub", "LE_n_def",
           "LE_n_real", "LE_n_ub", "RN_d_def", "RN_d_real", "RN_d_ub", "RN_n_def", "RN_n_real", "RN_n_ub",  
           "Tmax_d_d", "Tmax_d_r", "Tmax_d_u", "Tmax_n_d", "Tmax_n_r", "Tmax_n_u", "Tmean_d_d", "Tmean_d_r",
           "Tmean_d_u", "Tmean_n_d", "Tmean_n_r", "Tmean_n_u", "Tmin_d_d", "Tmin_d_r", "Tmin_d_u", "Tmin_n_d", 
           "Tmin_n_r", "Tmin_n_u")

for (name in names){

vals1 = seq(108.88161,124.16637,(124.16637-108.88161)/340)
vals2 = seq(28.60404,43.23213,(43.23213-28.60404)/410)

lon <- ncdim_def( name = 'longitude', units = 'degrees_east', vals1[1:340] )
lat <- ncdim_def( name = 'latitude', units = 'degrees_north', vals2[1:410])
t <- ncdim_def( name = 'time', units = 'years', vals = 0:18)

#放入变量
vars = ncvar_def(name, 'w/m2', dim = list(lon,lat,t), NA, longname= name, prec="float", shuffle=FALSE,
       compression=NA, chunksizes=NA, verbose=FALSE )
ncnew = nc_create(paste('K:/2 Irrigation+urbanization/data_process/T2m_sig/NCfile/withirr/',name,sep=""), vars, force_v4=FALSE, verbose=FALSE)
#ncvar_put(nc = ncnew, varid=vars, vals=Tmin_n_u)
ncvar_put(nc = ncnew, varid=vars, vals=get(name))
ncatt_put( nc = ncnew, varid = 0, attname = 'description', attval = name)
nc_close(ncnew)

print(name)

}


#打开检查
nc <- nc_open('K:/Surfex_Meteo_factors/Test/NCfile/def_test1.nc')
uu <- ncvar_get(nc, varid=vars, start=NA, count=NA, verbose=FALSE, signedbyte=TRUE, collapse_degen=TRUE)
