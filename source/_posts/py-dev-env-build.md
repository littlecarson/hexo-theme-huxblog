---
layout:     post
title:      "Python Enviroment Build"
subtitle:   "Python 项目开发环境搭建"
date:       2017-3-19 10:15:00
author:     "Carson"
catalog:    true
tags:
    - Python
---

> 基于阿里云平台 Ubuntu14.04 Server

## 开发依赖

```
sudo apt-get -y update    # update source
sudo apt-get -y upgrade   # upgrade
sudo apt-get -y install build-essential  # make requrie tool
sudo apt-get -y install libsqlite3-dev
sudo apt-get -y install libreadline6-dev
sudo apt-get -y install libgdbm-dev
sudo apt-get -y install zliblg-dev
sudo apt-get -y install libbz2-dev
sudo apt-get -y install sqlite3
sudo apt-get -y install tk-dev
sudo apt-get -y install zip
sudo apt-get -y libssl-dev
```

## 开发相关包
```
sudo apt-get -y install python-dev
```
## 编译安装 pyhton2.7.9
```
wget https://www.python.org/ftp/python/2.7.9/Python-2.7.9.tgz
tar xzvf Python-2.7.9.tgz
cd Python-2.7.9.tgz
LDFLAGS="-L/usr/lib/x86_64-linux-gnu" ./configure --prefix=/opt/python2.7.9
make
sudo make install
```

# install pip
```
wget https://bootstrap.pypa.io/get-pip.python-dev
sudo python get-pip.py
pip install virtualenv
pip freeze # 查看当前安装的包版本

# python2.7.9 后的版本自带 ensurepip 模块，	所以自带pip，可以执行 python -m ensurepip -U 更新到最新的pip版本
```

## 安装开发辅助
```
sudo pip install ipython  
```

