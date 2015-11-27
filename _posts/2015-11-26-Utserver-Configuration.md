---
title: utserver Configuration
layout: post
---



首先下载ubuntu下的安装包 ut.tar.gz

然后解压

    sudo tar -xvf ut.tar.gz
    
更改目录权限

    sudo chmod 777 -R ut/
    
移动到/opt

    sudo mv ut/ /opt/utorrent/
    
添加/utorrent/目录下的utserver软链接到/usr/bin/

    sudo ln -s /opt/utorrent/utserver /usr/bin/utserver
    
启动

    utserver -settingspath /opt/utorrent/ &
    

然后使用 youraddress:8080/gui/ 访问,默认密码为空

使用killall utserver来关闭utorrent

某些版本下可能需要安装包libssl0.9.8:i386

    sudo apt-get install libssl0.9.8:i386