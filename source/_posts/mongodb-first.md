---
layout:     post
title:      "MonogDB First"
subtitle:   "开始使用 Mongodb"
date:       2016-7-1 10:00:00
author:     "Carson"
catalog:    true
tags:
    - Monogdb
    - Pymongo
---

## Install

```
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927

# ubuntu 14
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

# ubuntu 16
echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

apt-get update

apt-get install -y mongodb-org

# 固定版本
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-org-shell hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections

# 启动MongoDB
sudo service mongod start
# 查看服务状态
sudo service mongod status

# 远程连接配置
vim /etc/mongod.conf
vim: #bind_ip 127.0.0.1 监听所有外网ip

# 下载管理脚本
wget https://github.com/mongodb/mongo/raw/master/debian/init.d
sudo mv init.d /etc/init.d/mongodb
sudo chmod +x /etc/init.d/mongodb

# 修改内核
sudo sh -c 'echo never > /sys/kernel/mm/transparent_hugepage/enabled'
sudo sh -c 'echo never > /sys/kernel/mm/transparent_hugepage/defrag'

# 卸载
sudo service mongod stop
sudo apt-get purge mongodb-org*
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb

# 查看配置文件
vim /etc/mongod.conf

#设置开机启动
sudo systemctl enable mongod

#取消开机启动
sudo systemctl disable mongod
```

## Python 下使用 MongoDB

### Install PyMongo

```
# Python2
pip install pymongo
# Python3
apt-get install python3-pip

# 安装GUN C compiler（GCC）（使用MongoDB的C扩展）
apt-get install build-essential python-dev

# 验证安装
python
> import pymongo
```

### 基本使用

```
# coding=utf-8

from pymongo import MongoClient

#  连接本地数据库
client = MongoClient()

#  选择数据库或创建数据库
database = client.school

#  选择数据集或创建集合
collection = database.student

# 创建一条字典格式数据
stu_dic = {'name': 'carson', 'age': 23, 'sex': 'male'}

# 插入一条数据（字典格式）
collection.insert(stu_dic)

# 查询集合中的所有数据
stu_all = colection.find() # 此处content是一个pymongo对象
for each in stu_all:
    print(each['name'])

# 查询指定的文档
content = col.find({'age': 29}) # 此处content是一个pymongo对象
content = col.find_one({'age': 23})

# 更新指定的数据
collection.update({'name': 'carson'}, {'age': 18})
collection.update_one({'age': 20}, {'$set':{'name': 'kingname'}})
collection.update_many({'age': 20}, {'$set':{'age': 30}})

# 删除指定数据
collection.delete_one({'name': 'kingname'})
collection.delete_many({'name': 'carson'})
```
