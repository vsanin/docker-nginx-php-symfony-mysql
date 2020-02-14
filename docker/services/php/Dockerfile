FROM php:7.4-fpm

RUN apt-get update && apt-get install -y \
        git \
        zip \
        unzip \
        libicu-dev \
        libbz2-dev \
        libpng-dev \
        libjpeg-dev \
        libmcrypt-dev \
        libreadline-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zlib1g-dev \
        libxml2-dev \
        libzip-dev \
        libonig-dev \
    && docker-php-ext-configure gd \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install zip \
    && docker-php-source delete

RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/5.1.1.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-5.1.1 /usr/src/php/ext/redis \
    && docker-php-ext-install redis

RUN     pecl install xdebug \
        && docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# Composer parallel install plugin
RUN composer global require hirak/prestissimo

COPY ./php.ini /usr/local/etc/php/php.ini
COPY ./www.conf /usr/local/etc/php-fpm.d/www.conf:ro
WORKDIR /code

RUN rm -rf /var/cache/apk/*

USER www-data

CMD ["php-fpm", "--nodaemonize"]
