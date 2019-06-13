
###版本选择
###@seen https://hub.docker.com/_/php?tab=description&page=2
ARG D_PHP_VERSION
FROM php:${D_PHP_VERSION}-fpm

LABEL maintainer="mallor <mallor@163.com>"

ARG D_PHP_VERSION
# always run apt update when start and after add new source list, then clean up at end.
RUN set -xe; \
      apt-get update -yqq && \
      pecl channel-update pecl.php.net && \
      apt-get install -yqq \
      apt-utils \
      libzip-dev zip unzip libmemcached-dev \
      libz-dev \
      libpq-dev \
      libjpeg-dev \
      libpng-dev \
      libfreetype6-dev \
      libssl-dev \
      libmcrypt-dev && \
      rm -rf /var/lib/apt/lists/* && \
      docker-php-ext-configure zip --with-libzip && \
      # Install the zip extension
      docker-php-ext-install zip && \
      docker-php-ext-install pdo_mysql && \
      docker-php-ext-install pdo_pgsql && \
      docker-php-ext-configure gd \
            --with-jpeg-dir=/usr/lib \
            --with-freetype-dir=/usr/include/freetype2 && \
      docker-php-ext-install gd && \
      docker-php-ext-install opcache && \
      php -m | grep -q 'zip'


#安装composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#安装第三方扩展
ARG D_PHP_SWOOLE

RUN if [ ${D_PHP_SWOOLE} ]; then \
    # Install Php Swoole Extension
   cd /tmp &&  curl -OL   https://github.com/swoole/swoole-src/archive/${D_PHP_SWOOLE}.tar.gz  && mkdir -p  /tmp/swoole &&  tar -zxvf ${D_PHP_SWOOLE}.tar.gz -C /tmp/swoole  --strip-components 1 \
      && ( \
        cd swoole/ \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r swoole* \
    && docker-php-ext-enable swoole \
;fi


ARG D_PHP_MONGODB

RUN if [ ${D_PHP_MONGODB} ]; then \
    # Install Php MONGODB Extension
   cd /tmp &&  curl -OL   https://github.com/mongodb/mongo-php-driver/releases/download/${D_PHP_MONGODB}/mongodb-${D_PHP_MONGODB}.tgz  && mkdir -p  /tmp/mongodb &&  tar -zxvf mongodb-${D_PHP_MONGODB}.tgz -C /tmp/mongodb  --strip-components 1 \
      && ( \
        cd mongodb/ \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r mongodb* \
    && docker-php-ext-enable mongodb \
;fi



ARG D_PHP_CPHALCON7

RUN if [ ${D_PHP_CPHALCON7} ]; then \
    # Install Php CPHALCON7 Extension
   cd /tmp &&  curl -OL   https://github.com/dreamsxin/cphalcon7/archive/${D_PHP_CPHALCON7}.tar.gz  && mkdir -p  /tmp/cphalcon7 &&  tar -zxvf ${D_PHP_CPHALCON7}.tar.gz -C /tmp/cphalcon7  --strip-components 1 \
      && ( \
        cd cphalcon7/ext \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r cphalcon7* \
    && docker-php-ext-enable phalcon \
;fi


ARG D_PHP_XHPROF

RUN if [ ${D_PHP_XHPROF} ]; then \
    # Install Php XHPROF Extension
   cd /tmp &&  curl -OL   https://github.com/tideways/php-xhprof-extension/archive/${D_PHP_XHPROF}.tar.gz  && mkdir -p  /tmp/xhprof &&  tar -zxvf ${D_PHP_XHPROF}.tar.gz -C /tmp/xhprof  --strip-components 1 \
      && ( \
        cd xhprof/ \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r xhprof* \
    && docker-php-ext-enable tideways \
;fi


ARG D_PHP_XDEBUG

RUN if [ ${D_PHP_XDEBUG} ]; then \
    # Install Php XDEBUG Extension
   cd /tmp &&  curl -OL   https://xdebug.org/files/xdebug-${D_PHP_XDEBUG}.tgz  && mkdir -p  /tmp/xdebug &&  tar -zxvf xdebug-${D_PHP_XDEBUG}.tgz -C /tmp/xdebug  --strip-components 1 \
      && ( \
        cd xdebug/ \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r xdebug* \
    && docker-php-ext-enable xdebug \
;fi



ARG D_PHP_VLD

RUN if [ ${D_PHP_VLD} ]; then \
    # Install Php VLD Extension
   cd /tmp &&  curl -OL   https://github.com/derickr/vld/archive/${D_PHP_VLD}.tar.gz  && mkdir -p  /tmp/vld &&  tar -zxvf ${D_PHP_VLD}.tar.gz -C /tmp/vld  --strip-components 1 \
      && ( \
        cd vld/ \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r vld* \
    && docker-php-ext-enable vld \
;fi


#增加php-fpm外部配置文件
RUN echo include=etc/php-fpm.d/cfg.d/*.conf >> /usr/local/etc/php-fpm.conf

#增加多php.ini配置目录
ENV PHP_INI_SCAN_DIR=/usr/local/etc/php/conf.d:/usr/local/etc/php/conf.d/mod

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

RUN usermod -u 1000 www-data


WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000