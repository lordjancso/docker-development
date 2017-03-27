FROM php:5.6-fpm
MAINTAINER lordjancso <lordjancso@gmail.com>

RUN apt-get update && apt-get install -y \
    libicu-dev \
    mysql-client \
    libmysqlclient-dev \
    libexif-dev \
    && docker-php-ext-install exif \
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

RUN apt-get install -y git
RUN git config --global alias.s "status -s"
RUN git config --global alias.lg "log --oneline --decorate --all --graph"

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

RUN apt-get install -y libmagickwand-dev \
    && pecl install imagick \
    && docker-php-ext-enable imagick

RUN cd ~ && git clone git://github.com/nicolasff/phpredis.git
RUN cd ~/phpredis && phpize && ./configure && make && make install

ADD config.ini /usr/local/etc/php/conf.d/

RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

RUN curl -L http://deployer.org/deployer.phar -o deployer.phar
RUN mv deployer.phar /usr/local/bin/dep
RUN chmod +x /usr/local/bin/dep

RUN curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony
RUN chmod a+x /usr/local/bin/symfony

RUN apt-get update
RUN apt-get install -y php5-dev php5-mysql gcc libpcre3-dev
RUN cd ~ && git clone --depth=1 git://github.com/phalcon/cphalcon.git
RUN cd ~/cphalcon/build && ./install

RUN cd ~ && git clone https://github.com/phalcon/phalcon-devtools.git
RUN cd ~/phalcon-devtools && ./phalcon.sh
RUN ln -s ~/phalcon-devtools/phalcon.php /usr/bin/phalcon
RUN chmod ugo+x /usr/bin/phalcon

RUN curl -L http://download.cdn.mozilla.net/pub/firefox/releases/46.0/linux-x86_64/en-GB/firefox-46.0.tar.bz2 -o ~/firefox.tar.bz2
RUN cd ~ && tar xjf firefox.tar.bz2
RUN mv ~/firefox/ /opt/firefox/
RUN ln -s /opt/firefox/firefox /usr/bin/firefox
RUN apt-get install -y libglib2.0-dev libgtk-3-dev
RUN apt-get --reinstall install -y libasound2

RUN apt-get install -y openjdk-7-jre-headless xvfb
RUN curl -L http://selenium-release.storage.googleapis.com/2.53/selenium-server-standalone-2.53.1.jar -o ~/selenium-server-standalone-2.53.1.jar
RUN Xvfb :10 -ac &
RUN export DISPLAY=:10
RUN nohup java -jar ~/selenium-server-standalone-2.53.1.jar >> nohup.out 2>&1 &

RUN curl -L https://github.com/FriendsOfPHP/PHP-CS-Fixer/releases/download/v1.12.2/php-cs-fixer.phar -o php-cs-fixer
RUN chmod a+x php-cs-fixer
RUN mv php-cs-fixer /usr/local/bin/php-cs-fixer

RUN usermod -u 1000 www-data
RUN echo "export TERM=xterm" >> /etc/bash.bashrc

WORKDIR /srv
