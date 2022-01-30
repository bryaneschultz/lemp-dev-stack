FROM php:7.4.11-fpm-buster

ARG DEBIAN_FRONTEND=noninteractive
ARG TIMEZONE

# Set default Timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
    echo ${TIMEZONE} > /etc/timezone

COPY php-fpm/conf/dev-local.ini /tmp/

RUN sed /tmp/dev-local.ini \
        -e "s|{%TIMEZONE%}|${TIMEZONE}|" \
        >  /usr/local/etc/php/php.ini

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libonig-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    libzip-dev \
    libicu-dev \
    autoconf

# Install extensions
RUN pecl install igbinary && \
    pecl install xdebug-3.1.2 && \
	pecl install -o -f redis && \
	rm -rf /tmp/pear && \
    docker-php-ext-enable xdebug && \
    docker-php-ext-enable redis && \
    docker-php-ext-install \
        pdo_mysql \
        mbstring \
        zip \
        exif \
        pcntl \
        intl && \
    docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg && \
    docker-php-ext-install gd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/local/bin \
    --filename=composer

# Clear cache
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*
