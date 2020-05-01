FROM php:7.2-fpm

RUN useradd -m john && echo "john:john" | chpasswd && adduser john sudo && adduser john www-data

ARG DEBIAN_FRONTEND=noninteractive

ENV HOME /home/john

RUN apt update \
        && apt install -y --no-install-recommends apt-utils

# Get repository and install wget and vim
RUN apt install --no-install-recommends -y \
        openssh-server \
        apt-utils \
        unzip \
        wget \
        cron \
        vim \
        git

# Install PHP extensions deps
RUN apt install -y --force-yes --no-install-recommends -y \
        libmagickwand-dev \
        libmemcached-dev \
        libfreetype6-dev \
        libmcrypt-dev \
        libjpeg-dev \
        libxslt-dev \
        libxml2-dev \
        libpng-dev \
        libssl-dev \
        libpq-dev \
        libz-dev

# Install Xdebug
RUN pecl install -f xdebug

# Install mycript
RUN pecl install mcrypt-1.0.2

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php \
	&& mv composer.phar /usr/local/bin/composer \
	&& composer global require hirak/prestissimo --no-plugins --no-scripts

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install pdo_pgsql
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install opcache
RUN docker-php-ext-install sockets
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install xmlrpc
RUN docker-php-ext-install iconv
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install intl
RUN docker-php-ext-install soap
RUN docker-php-ext-install exif
RUN docker-php-ext-install xsl
RUN docker-php-ext-install zip

# Enabling some php extensions
RUN docker-php-ext-enable bcmath
RUN docker-php-ext-enable mcrypt
RUN docker-php-ext-enable xdebug

# Install GD
RUN docker-php-ext-install gd

# Clean repository
RUN apt clean \
    && rm -rf /var/lib/apt/lists/*

# Copy ini files
COPY ./etc/php_ini/php72/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
COPY ./etc/php_ini/php.ini /usr/local/etc/php/php.ini

## Fixing owner permissions
RUN chown -R john:john $HOME

# Set user john
USER john