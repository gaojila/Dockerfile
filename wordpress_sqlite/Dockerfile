FROM ubuntu:18.04
MAINTAINER Red GaoJila <redgaojila@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
ENV DOCUMENT_ROOT /usr/share/nginx/html

#文件初始化
RUN mkdir

# 软件初始化
RUN apt-get update
RUN apt-get -y install nginx-full
RUN apt install -y php
RUN apt install -y php-fpm
RUN apt install -y php7.2-curl
RUN apt install -y php7.2-xml
RUN apt install -y php7.2-mysql
RUN apt install -y php7.2-gd
RUN apt install -y php7.2-imap
RUN apt install -y php7.2-splite
RUN apt install -y unzip curl
RUN apt install -y sqlite3 php7.2-sqlite
RUN apt install -y vim

RUN rm -rf ${DOCUMENT_ROOT}/*
RUN curl -o wordpress.tar.gz https://wordpress.org/latest.tar.gz
RUN tar -xzvf /wordpress.tar.gz --strip-components=1 --directory ${DOCUMENT_ROOT}

RUN curl -o sqlite-plugin.zip https://downloads.wordpress.org/plugin/sqlite-integration.1.8.1.zip /tmp/sqlite-integration.1.8.1.zip
RUN unzip sqlite-plugin.zip -d ${DOCUMENT_ROOT}/wp-content/plugins/
RUN rm sqlite-plugin.zip
RUN cp ${DOCUMENT_ROOT}/wp-content/plugins/sqlite-integration/db.php ${DOCUMENT_ROOT}/wp-content
RUN cp ${DOCUMENT_ROOT}/wp-config-sample.php ${DOCUMENT_ROOT}/wp-config.php

# 初始化php连接
RUN mkdir -p /run/php && touch /run/php/php7.2-fpm.sock && touch /run/php/php7.2-fpm.pid
RUN chown -R www-data.www-data ${DOCUMENT_ROOT}

COPY wordpress /etc/nginx/sites-available/default
RUN mkdir -p /etc/nginx/sites-enabled
RUN mkdir -p /etc/nginx/ssl
RUN rm /etc/nginx/sites-enabled/default
RUN ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/wordpress

EXPOSE 80
EXPOSE 443

# supervisor 守护进程
RUN apt-get install -y supervisor
RUN mkdir -p /var/log/supervisor
ADD supervisord.conf /etc/supervisor/supervisord.conf
ADD supervisor.conf /etc/supervisor/conf.d/wordpress.conf

CMD ["/usr/bin/supervisord","-n"]
