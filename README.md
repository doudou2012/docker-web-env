# docker-web-env

使用docker ，快速搭建nginx +php, Mysql , redis ,mongodb ,rabbitmq

一 nginx + php 
 说明
 php扩展  imagick curl redis yaf mongo OPCache 
 
 2 build
 cd /path/to/repo/nginx-php
 docker build -t webserver .  #构建nginx+php 
 3 run
   docker run -v /path/to/local/web/files:/var/www:rw -p 80:80 -d webserver /sbin/my_init --enable-insecure-key
二 Mysql
 1 说明
   mysql 5.5
 2 build
   docker build -t db/mysql 5.5/
 3 run mysql
   1. 普通的run 
      docker run -d -p 3306:3306 db/mysql
   2. 设置mysql密码
      docker run -d -p 3306:3306 -e MYSQL_PASS="mypass" db/mysql
   3. 挂载mysql数据库数据目录
      docker run -d -v /path/in/host:/var/lib/mysql db/mysql /bin/bash -c "/usr/bin/mysql_install_db"
   4. 主从复制
      主 docker run -d -e REPLICATION_MASTER=true -e REPLICATION_PASS=mypass -p 3306:3306 --name mysql db/mysql
      从 docker run -d -e REPLICATION_SLAVE=true -p 3307:3306 --link mysql:mysql db/mysql
  4 环境变量说明
     1 MYSQL_USER  数据库用户名  默认 admin
     2 MYSQL_PASS  用户名admin的密码
     3 STARTUP_SQL 启动初始化的时候，要执行的数据库  

