---
layout: post
title:  "Linux 安装 MySql"
date:   2017-01-11 16:50:02 +0800
categories: Java
tag: MySql
#excerpt: 
---

* content
{:toc}


#### 1、首先检查系统中是否已经安装了MySQL
    sudo netstat -tap | grep mysql





#### 2、如果没有安装，则安装MySQL.
    sudo apt-get install mysql-server mysql-client

#### 3、测试安装是否成功：
    sudo netstat -tap | grep mysql
	
#### 4、也可通过登录MySQL测试
    在终端输入 
	mysql -uroot -p 
	接下来会提示你输入密码，输入正确密码，即可进入。 

#### 5、MySQL的一些简单管理：
    启动MySQL服务： sudo start mysql
    停止MySQL服务： sudo stop mysql
    修改 MySQL 的管理员密码： sudo mysqladmin -u root password newpassword 
	
##### 设置远程访问(正常情况下，mysql占用的3306端口只是在IP 127.0.0.1上监听，拒绝了其他IP的访问（通过netstat可以查看到）。取消本地监 听需要修改 my.cnf 文件：)：
    sudo vi /etc/mysql/my.cnf
    bind-address = 127.0.0.1 //找到此内容并且注释
	
#### 6、MySQL安装后的目录结构分析(此结构只针对于使用apt-get install 在线安装情况)：
    数据库存放目录： /var/lib/mysql/
    相关配置文件存放目录： /usr/share/mysql
    相关命令存放目录： /usr/bin(mysqladmin mysqldump等命令)
    启动脚步存放目录： /etc/rc.d/init.d/
	
#### 7、MySQL图形化管理软件
- 一般使用的有两个比较好，一个开源，一个商业收费：
- 开源：MySQL Workbench （具体使用介绍随着我的使用，我会慢慢总结）
- 商业：Navicat Premium  （收费的，有30天体验，之后我相信大家会有办法的）
 
至此，Linux(Ubuntu)下的MyQL安装与配置全部完毕。