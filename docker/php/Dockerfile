FROM php:8.2-fpm

RUN apt-get update && apt-get install -y \
    zip unzip curl git \
    libpq-dev libzip-dev libonig-dev \
    nodejs npm \
    dos2unix \
    && docker-php-ext-install pdo pdo_mysql zip

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

COPY entrypoint.sh /usr/local/bin/entrypoint.sh
RUN dos2unix /usr/local/bin/entrypoint.sh && chmod +x /usr/local/bin/entrypoint.sh

WORKDIR /var/www

CMD ["php-fpm"]
