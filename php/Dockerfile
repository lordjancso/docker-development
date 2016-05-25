FROM php:5.6-fpm
MAINTAINER lordjancso <lordjancso@gmail.com>

RUN apt-get update && apt-get install -y \
    libicu-dev \
    mysql-client \
    libmysqlclient-dev \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install intl \
    && docker-php-ext-install opcache \
    && docker-php-ext-install zip \
    && docker-php-ext-install mbstring

RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libgd2-xpm-dev* \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

RUN apt-get install -y libmagickwand-dev \
    && pecl install imagick \
    && docker-php-ext-enable imagick

RUN apt-get install -y libmemcached-dev \
    && pecl install memcached \
    && docker-php-ext-enable memcached

RUN pecl install memcache \
    && docker-php-ext-enable memcache

ADD config.ini /usr/local/etc/php/conf.d/

RUN apt-get install -y git
RUN git config --global alias.s "status -s"
RUN git config --global alias.lg "log --oneline --decorate --all --graph"

RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

RUN curl -L http://deployer.org/deployer.phar -o deployer.phar
RUN mv deployer.phar /usr/local/bin/dep
RUN chmod +x /usr/local/bin/dep

RUN curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony
RUN chmod a+x /usr/local/bin/symfony

RUN apt-get update
RUN apt-get install -y php5-dev php5-mysql gcc libpcre3-dev
RUN git clone --depth=1 git://github.com/phalcon/cphalcon.git
RUN cd cphalcon/build && ./install

RUN usermod -u 1000 www-data
RUN echo "export TERM=xterm" >> /etc/bash.bashrc

WORKDIR /srv
