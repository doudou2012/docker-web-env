#说明
使用**docker**快速搭建**Nginx php mysql yaf redis memcache开发环境**
##Nginx+php
###版本
1. ***Nginx 1.9***
2. ***php 5.5.6***

###php扩展
***imagick curl redis yaf mongo OPCache***
###build 
1. ***cd /path/to/repo/nginx-php***
2. ***docker build -t webserver .***

###Run
***docker run -v /path/to/local/web/files:/var/www:rw -p 80:80 -d webserver /sbin/my_init --enable-insecure-key***

##Mysql
###版本
***Mysql 5.5.6*** 

###Build
***docker build -t db/mysql***
###Run
1. **普通Run: docker run -d -p 3306:3306 db/mysql**   
2. **设置mysql密码:docker run -d -p 3306:3306 -e MYSQL_PASS="mypass" db/mysql**
3. **挂载mysql数据库数据目录:docker run -d -v /path/in/host:/var/lib/mysql db/mysql /bin/bash -c "/usr/bin/mysql_install_db"**
4. **主从复制:** *主* **docker run -d -e REPLICATION_MASTER=true -e REPLICATION_PASS=mypass -p 3306:3306 --name mysql db/mysql**
5. **主从复制:** *从* **docker run -d -e REPLICATION_SLAVE=true -p 3307:3306 --link mysql:mysql db/mysql**

###变量说明
1. **MYSQL_USER 数据库用户名 默认root**
2. **MYSQL_PASS 数据库默认密码**





