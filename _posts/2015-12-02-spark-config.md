---
title: Spark Configuration
layout: post
---

# 配置一个spark集群

# 0.说明

本教程记录了我搭建一个spark集群的过程，我使用了两个虚拟机，一个作为master，一个作为worker。

注意本教程中的所有命令根据你的实际情况而定，请不要原样粘贴。

如果你对本教程有任何问题或者指出错误，请联系我。

# 1.准备工作

* Oracle VM VirtualBox
* ubuntu14.04 server
* ubuntu server已安装好java
* ubuntu server已安装好scala

# 2.hadoop配置

首先下载hadoop,我这里是hadoop-2.7.1.tar.gz，然后我把它解压到/usr/local目录，然后重命名并修改权限
	
	tar -vxzf hadoop-2.7.1.tar.gz -C /usr/local
	
	sudo mv hadoop-2.7.1.tar.gz hadoop
	
	sudo chmod 777 -R hadoop/
	
编辑 /etc/profile

	sudo vi /etc/profile

加入如下内容，注意将HADOOP_INSTALL的值换成你的安装目录

	export HADOOP_INSTALL=/usr/local/hadoop
	export PATH=$PATH:$HADOOP_INSTALL/bin
	export PATH=$PATH:$HADOOP_INSTALL/sbin
	export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
	export HADOOP_COMMON_HOME=$HADOOP_INSTALL
	export HADOOP_HDFS_HOME=$HADOOP_INSTALL
	export YARN_HOME=$HADOOP_INSTALL

现在开始编辑hadoop目录中的各配置文件，有些配置文件不存在，但是提供了相应的template文件	
	
编辑hadoop目录中的hadoop-env.sh文件

	sudo vi /usr/local/hadoop/etc/hadoop/hadoop-env.sh
	
加入如下内容,注意将JAVA_HOME的值换成你的JDK目录

	export JAVA_HOME=/usr/local/java/

编辑 core-site.xml文件

	sudo vi /usr/local/hadoop/etc/hadoop/core-site.xml
	
我的配置如下

	<configuration>

	<property>
	<name>fs.defaultFS</name>
	<value>hdfs://Master:9000</value>
	</property>

	/* 我将冲区设置为4KB */
	<property>
	<name>io.file.buffer.size</name>
	<value>131072</value>
	</property>

	/* 临时文件夹目录，注意修改成你的目录 */
	<property>
	<name>hadoop.tmp.dir</name>
	<value>file:/home/sr/tmp</value>
	</property>

	<property>
	<name>hadoop.proxyuser.hduser.hosts</name>
	<value>*</value>
	</property>

	<property>
	<name>hadoop.property.hduser.groups</name>
	<value>*</value>
	</property>
	</configuration>
	
编辑 yarn-site.xml文件

	sudo vi /usr/local/hadoop/etc/hadoop/yarn-site.xml
	
我的配置如下

	<configuration>

	<!-- Site specific YARN configuration properties -->
	<property>
	<name>yarn.nodemanager.aux-services</name>
	<value>mapreduce_shuffle</value>
	</property>

	<property>
	<name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
	<value>org.apache.hadoop.mapred.ShuffleHandler</value>
	</property>

	<property>
	<name>yarn.resourcemanager.address</name>
	<value>Master:8032</value>
	</property>

	<property>
	<name>yarn.resourcemanager.scheduler.address</name>
	<value>Master:8030</value>
	</property>

	<property>
	<name>yarn.resourcemanager.resource-tracker.address</name>
	<value>Master:8031</value>
	</property>

	<property>
	<name>yarn.resourcemanager.admin.address</name>
	<value>Master:8033</value>
	</property>

	<property>
	<name>yarn.resourcemanager.webapp.address</name>
	<value>Master:8088</value>
	</property>

	</configuration>
	
编辑 mapred-site.xml文件

	sudo vi /usr/local/hadoop/etc/hadoop/mapred-site.xml
	
我的配置如下

	<configuration>

	<property>
	<name>mapreduce.framework.name</name>
	<value>yarn</value>
	</property>

	<property>
	<name>mapreduce.jobhistory.address</name>
	<value>Master:10020</value>
	</property>

	<property>
	<name>mapreduce.jobhistory.webapp.address</name>
	<value>Master:19888</value>
	</property>

	</configuration>
	
编辑 hdfs-site.xml文件

	sudo vi /usr/local/hadoop/etc/hadoop/hdfs-site.xml
	
我的配置如下，注意其中的目录路径

	<configuration>
	
	<property>
	<name>dfs.namenode.secondary.http-address</name>
	<value>Master:9001</value>
	</property>

	/* 注意你的目录 */
	<property>
	<name>dfs.namenode.name.dir</name>
	<value>fiel:/home/sr/hadoop/hdfs/namenode</value>
	</property>

	/* 注意你的目录 */
	<property>
	<name>dfs.datanode.data.dir</name>
	<value>file:/home/sr/hdoop/hdfs/datanode</value>
	</property>

	<property>
	<name>dfs.replication</name>
	<value>3</value>
	</property>

	<property>
	<name>dfs.webhdfs.enabled</name>
	<value>true</value>
	</property>
	
编辑 Master文件

	sudo vi Master
	
我的配置如下

	master

注意，这里的master为主机名，需要在/etc/hosts中添加对应的ip地址

	sudo vi /etc/hosts

我添加的是master的地址

	192.168.56.108	master
	
按同样的方法配置slaves文件

然后在master节点上创建公钥，我均使用了空密码

	ssh-keygen -r rsa
	
然后将id_rsa.pub（在 ~/.ssh/ 目录下）放到其它所有worker节点上

	scp ~/.ssh/id_rsa.pub your_username@192.168.56.109:/home/your_username/tmp/
	
	cat /home/your_username/tmp/id_rsa.pub >> /home/your_username/.ssh/authorized_keys
	
格式化Namenode

	/usr/local/hadoop/bin/hadoop namenode -format
	
启动Hadoop
	
	/usr/local/hadoop/sbin/start-all.sh
	
在各个节点上通过jps查看各JVM进程是否启动，如果正常则配置成功

	jps

例如，我的master节点上显示

	3912 SecondaryNameNode
	4313 Jps
	4059 ResourceManager
	
worker节点上显示

	3057 Jps
	2934 NodeManager
	2799 DataNode

# 3.spark配置

首先下载spark,我这里是spark-1.5.2-bin-hadoop2.6.tgz，然后我把它解压到/usr/local目录，然后重命名并修改权限
	
	tar -vxzf spark-1.5.2-bin-hadoop2.6.tgz -C /usr/local
	
	sudo mv spark-1.5.2-bin-hadoop2.6 spark
	
	sudo chmod 777 -R spark/
	
编辑spark的spark-env.sh配置文件	

	sudo vi /usr/local/spark/conf/spark-env.sh

加入如下内容，注意各个目录和IP

	export JAVA_HOME=/usr/local/java
	export SCALA_HOME=/usr/local/scala
	export SPARK_WORKER_MEMORY=500mb
	export SPARK_MASTER_IP=192.168.56.108
	export MASTER=spark://192.168.56.108:7077
	
编辑spark的slaves配置文件

	sudo vi /usr/local/spark/conf/slaves

我的配置如下，同样注意这是主机名，记得像配置Hadoop的Master文件时那样修改hosts文件
	
	Slave1

启动Spark

	/usr/local/spark/sbin start-all.sh
	
同样使用jps查看JVM进程，会发现master节点上有Master，worker节点上有Worker
	
	




	