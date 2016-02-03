---
title: Caffe Configuration
layout: post
---

# 配置caffe环境

# 0.说明

本教程记录了我配置caffe环境的过程，我使用的是CPU_ONLY模式。

注意本教程中的所有命令根据你的实际情况而定，请不要原样粘贴。

如果你对本教程有任何问题或者指出错误，请联系我。

# 1.准备工作

* Oracle VM VirtualBox
* ubuntu14.04 server
* ubuntu server已安装好git

# 2.安装依赖项

首先安装BLAS

	sudo apt-get install libatlas-base-dev

其他依赖项

	sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev
	libboost-all-dev libhdf5-serial-dev protobuf-compiler liblmdb-dev libgflags-dev libgoogle-glog-dev

# 3.安装caffe

首先获得caffe源代码

	git clone https://github.com/BVLC/caffe.git
	
在caffe目录下，编辑配置文件，因为我使用CPU_ONLY模式，所以将配置文件中原本注释掉的 CPU_ONLY:= 1 取消注释

	cp Makefile.config.example Makefile.config
	
	vi Makefile.config
	
	// 然后将CPU_ONLY:= 1 取消注释
	
然后通过make编译

	make all
	
	make test
	
	make runtest
	
在runtest之后会输出PASSED的提示




	