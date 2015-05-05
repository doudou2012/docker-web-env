FROM phusion/baseimage:0.9.15
RUN locale-gen en_US.UTF-8
ENV LANG       en_US.UTF-8
ENV LC_ALL     en_US.UTF-8
ENV DEBIAN_FRONTEND noninteractive
RUN echo "deb http://www.rabbitmq.com/debian/ testing main" >> /etc/apt/sources.list && curl http://www.rabbitmq.com/rabbitmq-signing-key-public.asc | sudo apt-key add - 
ENV HOME /root
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh
CMD ["/sbin/my_init"]
# Nginx-PHP Installation
RUN apt-get update
RUN apt-get install -y vim curl wget build-essential python-software-properties  
RUN add-apt-repository -y ppa:ondrej/php5
RUN add-apt-repository -y ppa:nginx/stable
#php yaf
RUN add-apt-repository ppa:mikespook/php5-yaf
RUN apt-get update
RUN apt-get install -y --force-yes  imagemagick
RUN apt-get install -y --force-yes php-pear php5-dev  php5-cli php5-fpm php5-mysql php5-pgsql php5-sqlite php5-curl\
		       php5-gd php5-mcrypt php5-intl php5-imap php5-tidy php5-redis php5-imagick php5-yaf php5-memcached memcached 
RUN pecl install mongo amqp
#install opencc extension
RUN apt-get install -y --force-yes opencc libopencc-dev 
COPY ./opencc-php ./opencc-php
RUN cd opencc-php  && /usr/bin/phpize   
RUN cd opencc-php &&  ./configure  && make && make install
#install opencc end

RUN sed -i "s/;date.timezone =.*/date.timezone = UTC/" /etc/php5/fpm/php.ini
RUN sed -i "s/;date.timezone =.*/date.timezone = UTC/" /etc/php5/cli/php.ini
#config opencc extension
RUN echo "extension=opencc.so" >> /etc/php5/fpm/php.ini
RUN apt-get install -y nginx
RUN mkdir -p /etc/php5/conf.d
RUN echo "extension=mongo.so" >> /etc/php5/conf.d/mongo.ini
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php5/fpm/php-fpm.conf
RUN sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php5/fpm/php.ini

RUN mkdir -p        /var/www
ADD build/default   /etc/nginx/sites-available/default
RUN mkdir           /etc/service/nginx
ADD build/nginx.sh  /etc/service/nginx/run
RUN chmod +x        /etc/service/nginx/run
RUN mkdir           /etc/service/phpfpm
ADD build/phpfpm.sh /etc/service/phpfpm/run
RUN chmod +x        /etc/service/phpfpm/run

EXPOSE 80
# End Nginx-PHP

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
