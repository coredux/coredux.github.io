---
title: utserver Configuration
layout: post
---

#apache2 + php5 + mysql + yii 本地开发环境搭建

# 0.说明

我是在我的电脑(windows 8.1 pro)上通过虚拟机搭建的，我希望达到的目的是能够在虚拟机外编辑好代码，通过git提交到虚拟机里的仓库。

如果你对本教程有任何问题或者指出错误，请联系我。


# 1.准备工作

* Oracle VM VirtualBox
* ubuntu14.04 server
* Git for windows

# 2.开始

安装好虚拟机后，依次执行如下命令，安装好apache、mysql和php5以及它们所需要的库

	sudo apt-get install php5-cli

	sudo apt-get install apache2

	sudo apt-get install mysql-server

	sudo apt-get install libapache2-mod-php5 

	sudo apt-get install php5-mysql

	sudo apt-get install php5-gd

	sudo apt-get install php5-json
	
在安装 mysql-server时，曾出现错误
	
	mysql-server : Depends: mysql-server-5.5 but it is not going to be installed
	
使用 aptitude 安装得以解决

	sudo aptitude install mysql-server


然后再执行如下命令，全局安装一下composer

	curl -sS https://getcomposer.org/installer | php

	sudo mv composer.phar /usr/local/bin/composer

然后再安装mcrypt

	sudo apt-get install php5-mcrypt

如果能够在/etc/php5/conf.d/中找到mcrypt.ini，那么请将它移动到/etc/php5/mods-available/，如下，如果mcrypt.ini已经存在于/etc/php5/mods-available/，那么可以略过下面的命令

	sudo mv -i /etc/php5/conf.d/mcrypt.ini /etc/php5/mods-available/

运行一下mcrypt

	sudo php5enmod mcrypt

重启apache

	sudo service apache2 restart


安装Composer Asset插件

	sudo composer global require "fxp/composer-asset-plugin:1.0.0-beta3"

安装基本的yii应用程序模板

	sudo composer create-project yiisoft/yii2-app-basic basic 2.0.0


将basic复制到apache的DocumentRoot目录，我这里是/var/www/html

	sudo cp -R ~/basic /var/www/html/

有可能需要更改一下basic下runtime、assets、web/assets的权限

	sudo chmod 777 runtime/ assets/ web/assets

安装git

	sudo apt-get install git-core

如果没有安装ssh,安装ssh

	sudo apt-get install openssh-server
	sudo apt-get install openssh-client


在~/basic目录下初始化本地仓库
	
	sudo git init

在~目录下将它导出为裸仓库，我这里命名为basic.git

	sudo git clone --bare basic basic.git

我已经新建了一个没有root权限的用户,这里是git，用来在虚拟机外提交代码，现在新建一个群组，这里叫gitgrp，将git加入到gitgrp

	sudo groupadd gitgrp
	
	sudo usermod -a -G gitgrp git

然后变更一下basic.git的所属群组和权限

	sudo chgrp -R gitgrp ~/basic.git

	sudo chmod g+rwx -R ~/basic.git

在虚拟机外，clone这个仓库。注意，8990是我设置的虚拟机的端口转发（将8990转发到SSH的端口22），git是我加入到gitgrp的用户，/home/sr/是我放置basic.git的路径，你应该根据你的情况变更相应的参数

	sudo clone ssh://git@127.0.0.1:8990/home/sr/basic.git

在虚拟机内apache的DocumentRoot目录内的basic目录下（可以新建空目录），初始化git仓库

	sudo git init

添加本地的远程仓库，注意相应的参数

	sudo git remote add origin ssh://git@127.0.0.1/home/sr/basic.git

再pull一下

	sudo git pull origin master

这样，本地开发环境就搭建好了，你只需要在虚拟机外改动工程，然后提交到虚拟机的仓库，并且更新DocumentRoot下的工程。


