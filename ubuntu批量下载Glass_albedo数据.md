# python中生成批量下载链接，并且利用wget -i 批量下载Albedo数据

1）代码如下（以下载中国区域为例）
#!/usr/bin/python
a = []
for day in ['153', '161', '169', '177', '185', '193', '201', '209', '217', '225', '233', '241']:
    for band in ['h23v04', 'h23v05', 'h24v04', 'h24v05', 'h25v03', 'h25v04', 'h25v05', 'h25v06', 'h26v03', 'h26v04','26v05', 'h26v06', 'h27v04', 'h27v05', 'h27v06', 'h28v05', 'h28v06', 'h28v07', 'h28v08', 'h29v06',
                 'h29v07', 'h29v08']:

        a = "http://glass.umd.edu/Albedo/MODIS/1km"+"/2018/"+day+"/GLASS11A01.V42.A2018"+day+"."+band+".2019312.hdf"
        print(a)
        f = open(r'K:/Urbanization/Albedo2/test2018.txt', 'a')
        f.write(a+'\n')
        f.close()
结果如下，存入txt文件中：
http://glass.umd.edu/Albedo/MODIS/1km/2014/153/GLASS11A01.V42.A2014153.h23v04.2019312.hdf
http://glass.umd.edu/Albedo/MODIS/1km/2014/153/GLASS11A01.V42.A2014153.h23v05.2019312.hdf
http://glass.umd.edu/Albedo/MODIS/1km/2014/153/GLASS11A01.V42.A2014153.h24v04.2019312.hdf
http://glass.umd.edu/Albedo/MODIS/1km/2014/153/GLASS11A01.V42.A2014153.h24v05.2019312.hdf
http://glass.umd.edu/Albedo/MODIS/1km/2014/153/GLASS11A01.V42.A2014153.h25v03.2019312.hdf
http://glass.umd.edu/Albedo/MODIS/1km/2014/153/GLASS11A01.V42.A2014153.h25v04.2019312.hdf
http://glass.umd.edu/Albedo/MODIS/1km/2014/153/GLASS11A01.V42.A2014153.h25v05.2019312.hdf



2）利用ubuntu下的wget -i 命令
   例如： wget -i 2014.txt
   
