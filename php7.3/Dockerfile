FROM php:7.3-fpm-alpine

ENV PHPIZE_DEPS \
    autoconf \
    cmake \
    file \
    g++ \
    gcc \
    libc-dev \
    pcre-dev \
    make \
    git \
    pkgconf \
    re2c

RUN apk add --no-cache --virtual .persistent-deps icu-dev libzip-dev \
    && set -xe \
    && ln -s /usr/lib /usr/local/lib64 \
    && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS \
    && pecl install xdebug-2.7.0RC1 \
    && docker-php-ext-configure bcmath --enable-bcmath \
    && docker-php-ext-configure intl --enable-intl \
    && docker-php-ext-configure pcntl --enable-pcntl \
    && docker-php-ext-configure mysqli --with-mysqli \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql \
    && docker-php-ext-configure mbstring --enable-mbstring \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install -j$(nproc) \
        bcmath \
        intl \
        pcntl \
        mysqli \
        pdo_mysql \
        zip \
    && { find /usr/local/lib -type f -print0 | xargs -0r strip --strip-all -p 2>/dev/null || true; } \
    && apk del .build-deps \
    && rm -rf /tmp/* /usr/local/lib/php/doc/* /var/cache/apk/*
COPY config/php.ini /usr/local/etc/php/php.ini
COPY config/xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN apk --no-cache add shadow \
    && usermod -u 1000 www-data \
    && groupmod -g 1000 www-data \
    && apk del shadow

WORKDIR /srv
