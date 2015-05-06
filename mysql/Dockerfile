FROM phusion/baseimage:0.9.15
MAINTAINER fanzhanao<fanzhanao@gmail.com>
ENV DEBIAN_FRONTEND noninteractive

RUN echo "nameserver 8.8.8.8" | tee /etc/resolv.conf > /dev/null && echo "nameserver 8.8.8.8" | sudo tee /etc/resolvconf/resolv.conf.d/base > /dev/null

RUN echo "deb http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse" >> /etc/apt/sources.list.d/docker.list && \
echo "deb http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse" >> /etc/apt/sources.list.d/docker.list && \
echo "deb http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse" >> /etc/apt/sources.list.d/docker.list && \
echo "deb http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse" >> /etc/apt/sources.list.d/docker.list && \
echo "deb http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse" >> /etc/apt/sources.list.d/docker.list && \
echo "deb-src http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse" >> /etc/apt/sources.list.d/docker.list && \
echo "deb-src http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse" >> /etc/apt/sources.list.d/docker.list && \
echo "deb-src http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse" >> /etc/apt/sources.list.d/docker.list && \
echo "deb-src http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse" >> /etc/apt/sources.list.d/docker.list && \
echo "deb-src http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse" >> /etc/apt/sources.list.d/docker.list 

RUN apt-get update &&  apt-get -y --force-yes install mysql-server pwgen && \
  rm -rf /var/lib/apt/lists/*
#删除之前安装的mysql
RUN rm -rf /var/lib/mysql/*
#删除syslog配置
RUN rm /etc/mysql/conf.d/mysqld_safe_syslog.cnf
ADD my.cnf /etc/mysql/conf.d/my.cnf
ADD mysqld_charset.cnf /etc/mysql/conf.d/mysqld_charset.cnf

#mysql脚本
ADD import_sql.sh /import_sql.sh
ADD run.sh /run.sh
RUN chmod 755 /*.sh

ENV MYSQL_USER root 
ENV MYSQL_PASS modernmedia 
# Replication ENV
ENV REPLICATION_MASTER **False**
ENV REPLICATION_SLAVE **False**
ENV REPLICATION_USER replica
ENV REPLICATION_PASS replica

VOLUME  ["/etc/mysql", "/var/lib/mysql"]

EXPOSE 3306
CMD ["/run.sh"]
