FROM php:8.2-fpm

RUN apt-get update && apt-get install -y \
    libpq-dev \
    libzip-dev \
    libicu-dev \
    zip \
    unzip \
    git \
    curl \
    && docker-php-ext-install \
        pdo \
        pdo_pgsql \
        bcmath \
        zip \
        intl \
        opcache \
        pcntl \
        posix \
    && pecl install redis \
    && docker-php-ext-enable redis

# Setup OPcache configurations
COPY docker/php/opcache.ini /usr/local/etc/php/conf.d/opcache-custom.ini

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www