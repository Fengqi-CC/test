import numpy as np
import xarray as xr
import os, cmaps
import matplotlib.pyplot as plt
import cartopy.crs as ccrs
import cartopy.feature as cfeature
from cartopy.mpl.ticker import LongitudeFormatter, LatitudeFormatter
import cartopy.io.shapereader as shpreader
import shapefile
from matplotlib.patches import Polygon
from cartopy.util import add_cyclic_point
from scipy.stats.mstats import ttest_ind
from matplotlib.path import Path
from matplotlib.patches import PathPatch
from mpl_toolkits.basemap import Basemap
import maskout
import matplotlib.ticker as ticker

with xr.open_dataset("Tmax_d_u.nc") as f1:
    pre = f1['Tmax_d_u'][:,: ,:]
    lat, lon = f1['latitude'], f1['longitude']
    pre2d = np.array(pre).reshape(pre.shape[0],pre.shape[1] *pre.shape[2])
    #del pre

with xr.open_dataset("Tmax_d_d.nc") as f2:
    pre1 = f2['Tmax_d_d'][:,: ,:]
    lat, lon = f2['latitude'], f1['longitude']
    pre66 = np.array(pre1).reshape(pre1.shape[0],pre1.shape[1] *pre1.shape[2])
    #del pre


from scipy.stats.mstats import ttest_ind
t,p = ttest_ind(pre66, pre2d, 0, equal_var=True)
t1 = t.reshape(len(lat),len(lon))
p1 = p.reshape(len(lat),len(lon))
area = np.where(p < 0.1)


## 生成地图网格
pre_cor_cyc, lon_cyc = add_cyclic_point(t1, coord = lon)
nx, ny = np.meshgrid(lon_cyc, lat)
nx=nx[:,0:340]
ny=ny[:,0:340]

uu = np.mean((pre2d-pre66),0)
uu1 = uu.reshape(len(lat),len(lon))

fig = plt.figure(figsize=(14,18))
ax2 = plt.subplot(111, projection = ccrs.PlateCarree(central_longitude = 120))
ax2.set_xticks(np.arange(110,124,3), crs = ccrs.PlateCarree())
ax2.set_yticks(np.arange(30,43,3), crs = ccrs.PlateCarree())
ax2.xaxis.set_major_formatter(LongitudeFormatter(zero_direction_label = False))
ax2.yaxis.set_major_formatter(LatitudeFormatter())
ax2.coastlines(lw = 2)


#准备shp掩膜
shpfn = r'Wgs1984.shp'
reader = shpreader.Reader(shpfn)
wgsborder = shpreader.Reader('K:/2 Irrigation+urbanization/data_process/Drawing_python/Wgs1984')
statesFeat = cfeature.ShapelyFeature(reader.geometries(), crs = ccrs.PlateCarree(), facecolor='none')
ax2.add_feature(statesFeat, linewidth=1.5, edgecolor='k')

c2 = ax2.contourf(nx, ny, uu1, np.arange(-1, 1.2, 0.2), cmap = cmaps.BlWhRe, transform = ccrs.PlateCarree())

#masked out
maskout.shp2clip(c2, ax2, r'K:/2 Irrigation+urbanization/data_process/Drawing_python/country1', 'China')

## 显著性打点
nx2= np.array(nx).reshape(nx.shape[0]*nx.shape[1],)
ny2= np.array(ny).reshape(ny.shape[0]*ny.shape[1],)
ax2.scatter(nx2[area], ny2[area], marker = '.', s = 15, c = 'k', alpha = 0.4, transform = ccrs.PlateCarree())
u = plt.colorbar(c2, shrink = 1.2, pad = 0.04,fraction= 0.04)
u.ax.tick_params(labelsize=30)
for l in u.ax.yaxis.get_ticklabels():
        l.set_family('Times New Roman')
#u.set_label('colorbar',fontdict=font2)

# 设置坐标刻度值的大小以及刻度值的字体
plt.minorticks_on()
#plt.tick_params(labelsize=30,which='minor',width=2,length=8)
plt.tick_params(labelsize=30,which='major',width=4,length=20)
labels = ax2.get_xticklabels() + ax2.get_yticklabels()
[label.set_fontname('Times New Roman') for label in labels]

# 设置横纵坐标的名称以及对应字体格式
font2 = {'family': 'Times New Roman',
         'weight': 'normal',
         'size': 30,
         }
plt.xlabel('round', font2)
plt.ylabel('value', font2)
plt.title('Diff', fontsize = 30)

#plt.savefig('test69.png',dpi=600, bbox_inches = 'tight')
plt.savefig('test69.png',dpi=600)
plt.show()
