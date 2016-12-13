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

echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

apt-get update

apt-get install -y mongodb-org

# 启动MongoDB
service mongod start
# 查看服务状态
service mongod status

# 远程连接配置
vim /etc/mongod.conf
vim: #bind_ip 127.0.0.1 监听所有外网ip
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
