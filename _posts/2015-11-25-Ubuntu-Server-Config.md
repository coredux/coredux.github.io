---
title: ubuntu server Configuration
layout: post
---

# ����һ��ubuntu server�����

��ʹ��virtual box���������������ʹ��host-only �� �����ַת��(NAT) �ķ�ʽ

�������������鿴����������VirtualBox Host-Only Ethernet Adapter����������ַ

    192.168.56.1
    
Ȼ����virtual box��ȫ������������Host-only������ϸ

    IPv4��ַ��192.168.56.1
    IPv4 �������룺 255.255.255.0
    
����Ӧ��������������õ�����1����ΪHost-Onlyģʽ������2���ó������ַת��(NAT)ģʽ��ע��˳����֮�������ļ�������һ�¼���

�������������ip��ַ

    sudo vi /etc/network/interfaces
    
�ҵ��������£�ע��eth0��eth1��˳����ղ����������������һ��

    auto lo
    iface lo inet loopback
    
    auto eth0
    iface eth0 inet static
    address 192.168.56.103
    netmask 255.255.255.0
    network 192.168.56.0
    
    auto eth1
    iface eth1 inet dhcp

Ȼ��Ϳ�����������Pingͨ�����
 
    ping 192.168.56.103

Ϊ��ͨ��SSH���ӷ���������������а�װOpen-ssh
    
    sudo apt-get install openssh-server