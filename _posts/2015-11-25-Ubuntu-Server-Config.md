---
title: ubuntu server Configuration
layout: post
---

# 配置一个ubuntu server虚拟机

我使用virtual box虚拟机，网络设置使用host-only 加 网络地址转换(NAT) 的方式

首先在宿主机查看名字类似于VirtualBox Host-Only Ethernet Adapter的适配器地址

    192.168.56.1
    
然后再virtual box的全局设置中设置Host-only网络明细

    IPv4地址：192.168.56.1
    IPv4 网络掩码： 255.255.255.0
    
将对应虚拟机的网络设置的网卡1设置为Host-Only模式，网卡2设置成网络地址转换(NAT)模式，注意顺序与之后配置文件的设置一致即可

在虚拟机中配置ip地址

    sudo vi /etc/network/interfaces
    
我的配置如下，注意eth0与eth1的顺序，与刚才设置虚拟机的网卡一致

    auto lo
    iface lo inet loopback
    
    auto eth0
    iface eth0 inet static
    address 192.168.56.103
    netmask 255.255.255.0
    network 192.168.56.0
    
    auto eth1
    iface eth1 inet dhcp

然后就可以在宿主机Ping通虚拟机
 
    ping 192.168.56.103

为了通过SSH连接服务器，在虚拟机中安装Open-ssh
    
    sudo apt-get install openssh-server