---
title: phpmyadmin配置
layout: post
---

# yii开发环境搭建补充：在服务器上配置phpmyadmin

# 说明

本笔记的内容基于我的另一篇笔记[（apache2 + php5 + mysql + yii 本地开发环境搭建）](http://seanran.me/2015/11/29/Apache2-php5-mysql-yii-environment.html)，在操作数据库时，使用phpmyadmin可能会比较方便，本笔记的内容是在服务器上配置phpmyadmin

在本文中，文件的路径、IP地址、端口号与你的文件路径、IP地址、端口号可能有所不同，请不要原样复制命令

如果你对本教程有任何问题或者指出错误，请联系我。

# 配置

首先安装phpmyadmin

	sudo apt-get install phpmyadmin

然后将phpmyadmin中的apache配置文件复制到apache2/sites-available

	sudo cp /etc/phpmyadmin/apache.conf /etc/apache2/sites-available/phpmyadmin

然后建立到sites-enabled的软链接

	cd /etc/apache2/sites-enabled/  
	
	sudo ln -s ../sites-available/phpmyadmin

再建立phpmyadmin文件到服务器DocumentRoot的软链接

	sudo ln -s /usr/share/phpmyadmin/ /var/www/html/

重启apache

	sudo service apache2 restart

这样，就可以在虚拟机外通过浏览器使用phpmyadmin了

	http://192.168.56.1:8989/phpmyadmin