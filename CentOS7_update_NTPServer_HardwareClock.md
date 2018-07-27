服务器的时间快了9小时。

#date -R命令查看时区，正确。

#tzselect 重新选择时区
5 asia 9 china 1 beijingtime 1 yes
更改时区后时间仍然错误，不是时区的问题 。

#ntpdate向ntp服务器更新时间

显示no servers can be used, exiting

#ntpdate 218.186.3.36 显示需要sudo

#sudo -i

#ntpdate 218.186.3.36

更新系统时间成功，

adjust time server 218.186.3.36 offset XXXX( 9 hours) sec

以下步骤查看和矫正硬件时间

hwclock 查看硬件时间

hwclock -w 用系统时间同步硬件时间

以上可以将硬件时间矫正。


以下为参考命令

hwclock 有如下命令：

hwclock -r or  --show  显示时间    

hwclock -w or --systohc  
#用系统时间同步硬件时间    

hwclock -s or  --hctosys   #用硬件时间同步系统时间    

hwclock -a or --adjust  #矫正时间  

hwclock -v or --version   #工具版本    

hwclock --set --date=newdate  #设置时间  
