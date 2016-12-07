---
layout:     post
title:      "Build Environment by Vagrant"
subtitle:   "使用 Vagrant 搭建 Linux 开发环境"
date:       2016-9-27 10:00:00
author:     "Carson"
catalog:    true
tags:
    - Vagrant
---

## 安装 virtualbox 和 vagrant

安装 **virtualBox** 、**vagrant**

## 配置和添加 **box**

访问 https://atlas.hashicorp.com/boxes/search 选择需要的 **box**，查看想想描述和安装指令。(比如 `vagrant init ubuntu/trusty64;vagrant up`）


3.新建一个安装虚拟机目录
	`c:\>mkdir rails-va`
4.创建虚拟机初始化配置文件
	`c:\rails-va>vagrant init ubuntu/trusty64`
5.下载安装 `vagrant up`
	box中的镜像文件被放到了：/Users/username/.vagrant.d/boxes/；win放到：C:\Users\ 当前用户名\ .vagrant.d\boxes\目录下

## 本地配置box

1. 访问 http://www.vagrantbox.es/ 选择下载box到本地
2. 把下载好的box文件放到系统盘
3. 配置到vagrant: title为别名 url为box文件位置，在线本地的都可以
	```
	$ vagrant box add {title} {url}
	$ vagrant init {title}
	$ vagrant up
	// 本机为：vagrant box add centos7 c:\centos-7.0-x86_64.box	
	```
4. 移除box: `vagrant box remove box-name`

## 基本操作

1.修改内存:
	打开Vagrantfile添加

		config.vm.provider "virtualbox" do |v|
  			v.memory = 2048
		end
	执行vagrant reload
2.添加自定义用户，登录虚拟机:

	sudo adduser peter --ingroup sudo

3.虚拟机操作

    查看虚拟机的状态：vagrant status
	虚拟机关机：vagrant halt
	虚拟机挂起暂停: vagrant suspend
    虚拟机恢复执行：vagrant resume
	删除虚拟机：vagrant destroy


### 我的rails虚拟机安装配置

```
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "ubun"
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
  end
  
  config.vm.network :private_network, ip: "192.168.10.10"
```

### 打包虚拟机环境

```
vagrant up
vagrant ssh
sudo rm -rf /etc/udev/rules.d/70-persistent-net.rules
exit
vagrant package
ls >>> package.box
vagrant box add carson/ubuntu14 package.box
rm -rf package.box
vagrant box list
```
