FROM php:5.6-fpm
MAINTAINER lordjancso <lordjancso@gmail.com>

RUN apt-get update && apt-get install -y \
	libmcrypt-dev  \
	libicu-dev \
	mysql-client \
	libmysqlclient-dev \
	libgd2-xpm-dev* \
	&& docker-php-ext-install mysqli \
	&& docker-php-ext-install pdo_mysql \
	&& docker-php-ext-install iconv \
	&& docker-php-ext-install mcrypt \
	&& docker-php-ext-install intl \
	&& docker-php-ext-install gd \
	&& docker-php-ext-install opcache \
	&& docker-php-ext-install mbstring

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

ADD symfony.ini /usr/local/etc/php/conf.d/

RUN usermod -u 1000 www-data
