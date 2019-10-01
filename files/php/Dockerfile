FROM php:7.3-fpm-stretch
LABEL maintainer="gzp@goozp.com"


# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENV DEBIAN_FRONTEND noninteractive

# set apt source
# RUN echo "deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse " > /etc/apt/sources.list
# RUN echo "deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse " >> /etc/apt/sources.list
# RUN echo "deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse " >> /etc/apt/sources.list
# RUN echo "deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse " >> /etc/apt/sources.list
# RUN echo "deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse " >> /etc/apt/sources.list
# RUN echo "deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse " >> /etc/apt/sources.list
# RUN echo "deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse " >> /etc/apt/sources.list
# RUN echo "deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse " >> /etc/apt/sources.list
# RUN echo "deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse " >> /etc/apt/sources.list
# RUN echo "deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse" >> /etc/apt/sources.list

# RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3B4FE6ACC0B21F32    
RUN sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list && rm -Rf /var/lib/apt/lists/*

# Install package and PHP Core extensions
RUN apt-get update --fix-missing && apt-get install -y --no-install-recommends \
        dirmngr \
        apt-utils \
        gnupg \ 
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        vim \
        libzip-dev zip \
        wget nodejs \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-configure zip --with-libzip \
        && docker-php-ext-install zip \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli \
        && pecl install Xdebug \
        && docker-php-ext-enable xdebug \
        && pecl install swoole \
        && docker-php-ext-enable swoole 

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - && apt-get install -y nodejs && npm install --registry=https://registry.npm.taobao.org

#xdebug
# RUN echo "xdebug.remote_enable = 1" >> /usr/local/etc/php/php.ini
# RUN echo "xdebug.idekey = PHPSTORM" >> /usr/local/etc/php/php.ini
# RUN echo "xdebug.remote_host=127.0.0.1" >> /usr/local/etc/php/php.ini
# RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/php.ini

# Prevent error in nginx error.log
RUN touch /var/log/xdebug_remote.log
RUN chmod 777 /var/log/xdebug_remote.log

# Copy extensions had downloaded
COPY ./pkg/redis.tgz /home/redis.tgz
# COPY ./pkg/cphalcon.tar.gz /home/cphalcon.tar.gz

# Install PECL extensions (Redis)
RUN pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# Install Composer
#ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer config -g repo.packagist composer https://packagist.phpcomposer.com
RUN mkdir -p /var/www/.composer && chmod 777 -R /var/www/.composer
#ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

RUN rm -f /home/redis.tgz \
        rm -f /home/cphalcon.tar.gz 

WORKDIR /data/www

# Write Permission
RUN usermod -u 1000 www-data
# set user bash shell
RUN usermod -s /bin/bash www-data
