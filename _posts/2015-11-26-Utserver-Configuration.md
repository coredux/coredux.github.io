---
title: utserver Configuration
layout: post
---



��������ubuntu�µİ�װ�� ut.tar.gz

Ȼ���ѹ

    sudo tar -xvf ut.tar.gz
    
����Ŀ¼Ȩ��

    sudo chmod 777 -R ut/
    
�ƶ���/opt

    sudo mv ut/ /opt/utorrent/
    
���/utorrent/Ŀ¼�µ�utserver�����ӵ�/usr/bin/

    sudo ln -s /opt/utorrent/utserver /usr/bin/utserver
    
����

    utserver -settingspath /opt/utorrent/ &
    

Ȼ��ʹ�� youraddress:8080/gui/ ����,Ĭ������Ϊ��

ʹ��killall utserver���ر�utorrent

ĳЩ�汾�¿�����Ҫ��װ��libssl0.9.8:i386

    sudo apt-get install libssl0.9.8:i386