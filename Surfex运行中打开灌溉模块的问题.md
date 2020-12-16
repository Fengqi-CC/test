# Surfex运行中打开灌溉模块的问题

1.正确的做法是，在生成namelist的脚本文件（make_pgd_923，job_pgd_4km_3H_irr2.sh，不包括job_923）中修改OPTIONS.nam 下面的内容，
添加一行：
/
 &NAM_AGRI
 LAGRIP=.TRUE.,
 /
2.在namelist的EXSEG1.nam文件中不需要做任何修改/不需要添加灌溉。 
3.PRE_REAL1.nam也不需要添加灌溉。
 

