---
title: 在非root用户下安装mpi4py
categories: Note
---

### step1:
安装mpi4py所需要的依赖包(python2.7版本/Cpython/Openmpi)
1.源码包安装Python2.7版本
```
./configure prefix="#python安装目录(绝对路径)"
make
make install
```
2.安装Cpython
使用当前用户目录下的python版本来进行安装
```
/home/XXX/python27/bin/python setup.py install
```

3.安装openmpi
```
./configure prefix="#openmpi安装目录(绝对路径)"
make
make install
```
### step3:
配置openmpi环境变量
```
vim ~/.bashrc
# ~/.bashrc末尾添加
export PATH=#openmpi的绝对路径/bin:$PATH
soucre ~/.bashrc
```

### step4:
安装mpi4py包(同Cpython包安装方法)
```
/home/XXX/python27/bin/python setup.py install
```



