---
title: Theano windows installation tips (GPU)
layout: post
---

# 在windows下安装theano with GPU

# 0.说明

本教程记录了我在Windows下安装theano（使用GPU）的过程,由于整个过程踩坑无数,所以只大概几下了一些可能的坑

注意本教程中的所有命令根据你的实际情况而定，请不要原样粘贴。

如果你对本教程有任何问题或者指出错误，请联系我。

# 1.准备工作

* Cuda toolkit 8.0
* Cudnn-8.0

# 2.创建.theanorc.txt

在合适位置创建一个.theanorc.txt,然后在anaconda的目录下修改configparser.py

我的configparser.py位置在
	
	C:\Anaconda2\Lib\site-packages\theano
	
之所以修改它是因为我发现我在Users\me\下创建.theanorc.txt后, configparser.py中默认的 ~/.theanorc.txt并没有被找到

.theanorc.txt的内容(注意各个目录)
	
	[global]
	openmp=False 
	device = gpu   
	optimizer_including=cudnn  
	floatX = float32  
	allow_input_downcast=True  
	[lib]
	cnmem = 0.8 
	[blas]
	ldflags=  
	[gcc]
	cxxflags=-ID:\Anaconda2\MinGW  
	[nvcc]
	fastmath = True  
	--flags=-LD:\Anaconda2\libs
	--compiler_bindir=C:\Program Files\Microsoft Visual Studio 12.0\VC\bin
	

# 3.检查环境变量（Path）

	C:\Anaconda2;
	C:\Anaconda2\Scripts;
	C:\Anaconda2\MinGW\bin;
	C:\Anaconda2\MinGW\x86_64-w64-mingw32\lib;

# 4.检查环境变量（PYTHONPATH）

	C:\Anaconda2\Lib\site-packages\theano;
	
# 5.import theano测试

我又添加了如下环境变量才解决

	INCLUDE: C:\Program Files (x86)\Windows Kits\10\Include\10.0.10150.0\ucrt
	LIB: C:\Program Files (x86)\Microsoft SDKs\Windows\v7.1A\Lib\x64;C:\Program Files (x86)\Windows Kits\10\Lib\10.0.10150.0\ucrt\x64
	
	





	