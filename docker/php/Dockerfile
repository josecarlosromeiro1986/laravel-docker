FROM php:8.4-fpm

# Install Dependencies
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libicu-dev \
    libpq-dev \
    locales \
    libcurl4-openssl-dev \
    zip \
    unzip \
    curl

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Install and enable Xdebug extension for PHP
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Install and enable Redis extension for PHP
RUN pecl install redis \
    && docker-php-ext-enable redis

# Install some PHP extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure intl \
    && docker-php-ext-install \
    gd \
    mysqli \
    pdo_mysql \
    xml \
    curl \
    mbstring \
    zip \
    intl \
    bcmath \
    soap \
    opcache

# Set locale to pt_BR.UTF-8
RUN locale-gen pt_BR.UTF-8

# Add Composer global bin to PATH
ENV PATH="/root/.composer/vendor/bin:${PATH}"

# Install Laravel Installer
RUN composer global require laravel/installer

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /var/www

# Start PHP-FPM server
CMD ["php-fpm"]