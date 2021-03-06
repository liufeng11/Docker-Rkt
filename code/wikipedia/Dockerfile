FROM ubuntu:14.04
MAINTAINER Shivam Maharshi <shivam.maharshi@gmail.com>

LABEL Description="This image provides a full fledged MediaWiki runtime environment on Ubuntu 14.04." Vendor="Shivam Maharshi" Version="1.0"

RUN apt-get update && apt-get install -y \
        git \
        htop \
        vim \
        curl \
        wget 

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get install -y --force-yes \
        apache2 \
        php5 \
        libapache2-mod-php5 \
        mysql-server \
        mysql-client \
        php5-mysql \
        phpmyadmin

RUN service apache2 start
RUN service mysql start
RUN export TERM=xterm
RUN wget "https://releases.wikimedia.org/mediawiki/1.26/mediawiki-1.26.2.tar.gz"
RUN tar -xvzf mediawiki-1.26.2.tar.gz
RUN mv mediawiki-1.26.2 /var/www/wiki
RUN echo "nameserver 8.8.8.8" >> /etc/resolv.conf # For rkt
RUN echo "Include /etc/phpmyadmin/apache.conf" >> /etc/apache2/apache2.conf
RUN echo "ServerName 127.0.0.1" >> /etc/apache2/apache2.conf
RUN rm /etc/apache2/ports.conf
RUN git clone https://github.com/shivam-maharshi/Docker-MediaWiki
RUN cp /Docker-MediaWiki/Ubuntu-14.04/ports.conf /etc/apache2/ports.conf
ENV PORT 8080
RUN echo "Listen $PORT" >> /etc/apache2/ports.conf
RUN echo "$cfg['Servers'][$i]['user'] = 'root';" >> /etc/phpmyadmin/config.inc.php
RUN echo "$cfg['Servers'][$i]['AllowNoPasswordRoot'] = true;" >> /etc/phpmyadmin/config.inc.php
EXPOSE $PORT
#RUN mysql -u root -e 'CREATE DATABASE wiki;'
#RUN mysql -u root -e 'GRANT ALL PRIVILEGES ON wiki.* TO 'root'@'127.0.0.1';'
RUN sh Docker-MediaWiki/Ubuntu-14.04/install-mw.sh
