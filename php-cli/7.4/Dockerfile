FROM php:7.4-cli

COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN apt-get update \
    && apt-get install -y git zlib1g-dev libzip-dev unzip wget\
    && docker-php-ext-install zip pcntl

RUN echo 'memory_limit = -1' > /usr/local/etc/php/conf.d/php-unlimit-memory.ini

COPY ./ /app

ENV COMPOSER_ROOT_VERSION=0.12.9

RUN composer install --working-dir /app
