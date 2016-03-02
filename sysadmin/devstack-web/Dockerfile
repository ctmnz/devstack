FROM php:7.0.3-apache

ENV PHP_EXTRA_CONFIGURE_ARGS --with-curl  --enable-opcache 

## Apache confs
ADD apache-confs/*.conf /etc/apache2/sites-enabled/
#ADD apache-confs/default-ssl.conf /etc/apache2/sites-enabled/ 

## SSL certificates
#ADD apache-certs/apache.crt /etc/ssl/certs/apache.crt
#ADD apache-certs/apache.key /etc/ssl/certs/apache.key
ADD apache-certs/apache.crt /etc/ssl/certs/apache.crt
ADD apache-certs/apache.key /etc/ssl/certs/apache.key

## apache mods
RUN a2enmod rewrite headers deflate expires env vhost_alias ssl

RUN apt-get update -y && apt-get upgrade -y
RUN apt-get install -y git zlib1g-dev libmemcached-dev libmagickwand-dev  

# RUN docker-php-ext-install phpunit
RUN docker-php-ext-install pdo_mysql mysqli mbstring soap gd

RUN docker-php-ext-install opcache

#RUN apt-get update && apt-get install -y libmemcached-dev \
#    && pecl install memcached \
#    && docker-php-ext-enable memcached

#RUN pecl install -f imagick-3.4.0 \
#    && docker-php-ext-enable imagick


RUN pecl install -f zip \
    && docker-php-ext-enable zip


RUN git clone https://github.com/php-memcached-dev/php-memcached /usr/src/php/ext/memcached \
  && cd /usr/src/php/ext/memcached && git checkout -b php7 origin/php7 \
  && docker-php-ext-configure memcached \
  && docker-php-ext-install memcached 

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

#RUN \
#  apt-get update && \
#  apt-get -y install \
#     imagemagick

RUN \
  apt-get clean

