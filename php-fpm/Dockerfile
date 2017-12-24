FROM php:5.6-fpm
MAINTAINER idoplay <hlxo@hotmail.com>


RUN curl -s http://getcomposer.org/installer | php && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng12-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
    && pecl channel-update pecl.php.net \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update --fix-missing && apt-get install -y \
    g++ autoconf bash git apt-utils libxml2-dev libcurl3-dev pkg-config \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone

RUN docker-php-ext-install mcrypt \
    # Install the PHP pdo_mysql extention
    && docker-php-ext-install mysql \
    && docker-php-ext-install pdo_mysql \
    # Install the PHP pdo_pgsql extention
    && docker-php-ext-install pdo_pgsql \
    # Install the PHP gd library
    && docker-php-ext-configure gd \
        --enable-gd-native-ttf \
        --with-jpeg-dir=/usr/lib \
        --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd \
    && docker-php-ext-configure curl \
    && docker-php-ext-configure xml \
    && docker-php-ext-configure zip \
    && docker-php-ext-configure iconv \
    && docker-php-ext-configure json \
    && docker-php-ext-configure mbstring

RUN curl -L -o /tmp/memcached.tar.gz "http://pecl.php.net/get/memcached-2.2.0.tgz" \
    && mkdir -p memcached \
    && tar -C memcached -zxvf /tmp/memcached.tar.gz --strip 1 \
    && ( \
        cd memcached \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r memcached \
    && rm /tmp/memcached.tar.gz \
    && docker-php-ext-enable memcached


RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

RUN apt-get update -yqq && \
    apt-get -y install libxml2-dev php-soap && \
    docker-php-ext-install soap

#----Dev
#RUN pecl install xdebug && \
#    docker-php-ext-enable xdebug \
#COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini



ADD ./site.ini /usr/local/etc/php/conf.d
ADD ./site.pool.conf /usr/local/etc/php-fpm.d/

RUN rm -rf /var/lib/apt/lists/*

RUN usermod -u 1000 www-data

#WORKDIR /var/www
VOLUME /var/www

CMD ["php-fpm"]

EXPOSE 9000
