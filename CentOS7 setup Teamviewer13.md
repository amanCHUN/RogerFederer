CentOS7 安装Teamviewer13

官方下载最新的安装包
这里下载的版本teamviewer_13.1.3026.x86_64.rpm

安装之前yum install update

yum install teamviewer_13.1.3026.x86_64.rpm
显示缺少依赖包libQt5WebKit.so.5和libQt5WebKit.so.5

下载依赖包
https://pkgs.org/download/libQt5WebKitWidgets.so.5%28%29%2864bit%29
qt5-qtwebkit-5.6.2-1.el7.x86_64.rpm


安装teamviewer
sudo –i
yum install teamviewer_13.1.3026.x86_64.rpm

默认的安装位置
/opt/teamviewer/
cd /opt/teamviewer/tv_bin/

输入远程控制的初始密码
teamviewer passwd water9rock

设置后teamviewer自动运行，并且开机自动运行。
teamviewer info 可以查看用户ID和密码




