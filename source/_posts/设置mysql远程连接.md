---
title: 设置mysql远程连接
date: 2019-09-07 15:35:08
tags: [Mysql] 
categories: "Mysql"
---

### 在linux中安装完mysql服务器后一般需要设置远程链接
使用mysql图形化工具navicat,点击连接，填写账号密码。

出现报错
检查服务防火墙是否关闭，
3306端口是否可以访问

<!--more-->

进入服务器，登录mysql
```
[root@izbp1exnqsdfhdzz24q8dgz ~]# sudo mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 61
Server version: 5.1.73-log Source distribution

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```
二：进入mysql数据库，修改user表
```
#进入mysql数据库
use mysql
#修改user表设置root用户任何IP都可以访问
update user set host = '%' where user = 'root';
#刷新该表
frush privileges;
```
现在通过Navicat应该可以连接

如果还报错的话并且错误码为10061的话可能mysql服务器默认绑定127.0.0.1端口

修改mysql配置文件
 ```
 #这个是ubuntu的mysql文件地址
 vi /etc/mysql/mysql.conf.d/mysqld.cnf 
```
找到如下内容
```
#
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.

#将bind-address 127.0.0.1注释掉就好了
#bind-address           = 127.0.0.1
```
最后重启mysql服务器
```
systemctl restart mysql
```
ok，现在应该可以连接成功


<!-- # 使用 pv 记录方式，每访问一次，记录一次--> 
<span id="busuanzi_container_page_pv">  本文总阅读量<span id="busuanzi_value_page_pv"></span>次</span>