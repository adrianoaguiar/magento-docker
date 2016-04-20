# php:7.0.4-fpm
# xdebug 2.4.0
# web root at /var/www/html/magento
FROM php:7.0.4-fpm

# prepare magento dir
RUN mkdir -p /var/www/html/magento

# prepare php-fpm conf
ENV USR_LOCAL /usr/local
ENV LOCAL_ETC $USR_LOCAL/etc
ENV PHP_FPM_DIR $LOCAL_ETC/php-fpm.d
ENV PHP_INI_DIR $LOCAL_ETC/php
RUN rm $PHP_FPM_DIR/zz-docker.conf
#RUN rm $PHP_FPM_DIR/*.default
RUN rm $PHP_FPM_DIR/www.conf
COPY ./www.conf $PHP_FPM_DIR/

# Debug
# RUN apt-get update && apt-get install libfcgi0ldbl

# install php7 extensions for magento
COPY docker-php-ext-* /usr/local/bin/

RUN chown -R www-data:www-data /var/www/html

RUN apt-get update && apt-get dist-upgrade -y && apt-get install -y wget git
#RUN apt-get install -y software-properties-common
#RUN LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php
RUN apt-get update && apt-get install -y vim libmcrypt-dev libmemcached-dev libgearman-dev libfreetype6-dev libjpeg62-turbo-dev libpng12-dev libxml2-dev libcurl4-gnutls-dev libpspell-dev zlib1g-dev libbz2-dev libicu-dev g++ mlocate
RUN docker-php-ext-configure gd --with-jpeg-dir=/usr/lib/x86_64-linux-gnu
RUN docker-php-ext-install gd mysqli pdo pdo_mysql mcrypt intl mbstring soap curl xml pspell bz2 bcmath


# Redis for php
# Currently not available through PECL :-(
RUN cd /etc && git clone --depth=1 -b php7 https://github.com/phpredis/phpredis.git \
    && cd /etc/phpredis \
    && phpize \
    && ./configure \
    && make \
    && make install \
    && touch $PHP_INI_DIR/conf.d/ext-redis.ini \
    && echo 'extension=redis.so' >> $PHP_INI_DIR/conf.d/ext-redis.ini

# apt-get install xdebug 2.4.0
RUN touch $PHP_INI_DIR/conf.d/xdebug.ini; \
    echo xdebug.remote_enable=1 >> $PHP_INI_DIR/conf.d/xdebug.ini; \
    echo xdebug.remote_autostart=0 >> $PHP_INI_DIR/conf.d/xdebug.ini; \
    echo xdebug.remote_connect_back=1 >> $PHP_INI_DIR/conf.d/xdebug.ini; \
    echo xdebug.remote_port=9888 >> $PHP_INI_DIR/conf.d/xdebug.ini; \
    echo xdebug.remote_log=/tmp/php5-xdebug.log >> $PHP_INI_DIR/conf.d/xdebug.ini;

RUN mkdir ~/software && \
    cd  ~/software/ && \
    apt-get install -y wget && \
    wget http://xdebug.org/files/xdebug-2.4.0.tgz && \
    tar -xvzf xdebug-2.4.0.tgz && \
    cd xdebug-2.4.0 && \
    phpize && \
    ./configure && \
    make && \
    cp modules/xdebug.so /usr/local/lib/php/extensions/no-debug-non-zts-20151012 && \
    echo "zend_extension = /usr/local/lib/php/extensions/no-debug-non-zts-20151012/xdebug.so" >>  $PHP_INI_DIR/php.ini

# xdebug
RUN echo "max_execution_time = 3600" >> $PHP_INI_DIR/php.ini
RUN echo "default_socket_timeout = 3600" >> $PHP_INI_DIR/php.ini

# Java
RUN apt-get update && apt-get install -y openjdk-7-jre
ENV JAVA_HOME /usr/lib/jvm/java-7-openjdk-amd64

EXPOSE 9999
CMD ["php-fpm"]

