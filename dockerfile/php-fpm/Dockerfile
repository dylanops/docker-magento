FROM php:8.1.7-fpm

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=2.1.3

# Locales
RUN apt-get update \
	&& apt-get install -y locales

RUN dpkg-reconfigure locales \
	&& locale-gen C.UTF-8 \
	&& /usr/sbin/update-locale LANG=C.UTF-8

RUN echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen \
	&& locale-gen

ENV LC_ALL C.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# Common
RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		openssl \
		git \
		gnupg2 \
		vim \
		telnet \
		cron

# Install php extension

# intl
RUN apt-get install -y libicu-dev \
	&& docker-php-ext-configure intl \
	&& docker-php-ext-install -j$(nproc) intl

# xml
RUN apt-get install -y \
	libxml2-dev \
	libxslt-dev \
	&& docker-php-ext-install -j$(nproc) \
		dom \
		xsl \
		simplexml

# images
RUN apt-get install -y \
	libfreetype6-dev \
	libjpeg62-turbo-dev \
	libpng-dev \
	libgd-dev \
	&& docker-php-ext-configure gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) \
		gd \
		exif

# database
RUN docker-php-ext-install -j$(nproc) \
	mysqli \
	pdo \
	pdo_mysql

# strings
RUN apt-get install -y libonig-dev \
	&& docker-php-ext-install -j$(nproc) \
	gettext \
	mbstring

# math
RUN apt-get install -y libgmp-dev \
	&& ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h \
	&& docker-php-ext-install -j$(nproc) \
		gmp \
		bcmath

# compression
RUN apt-get install -y \
	libbz2-dev \
	zlib1g-dev \
	libzip-dev \
	&& docker-php-ext-install -j$(nproc) \
		zip \
		bz2

# memcached
RUN apt-get install -y \
	libmemcached-dev \
	libmemcached11

# curl
RUN apt-get install -y \
	libcurl4 \
	libcurl4-openssl-dev \
	&& docker-php-ext-install -j$(nproc) curl

# images
RUN apt-get install -y \
	libfreetype6-dev \
	libjpeg62-turbo-dev \
	libpng-dev \
	libgd-dev \
	&& docker-php-ext-configure gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) \
		gd \
		exif

# others
RUN docker-php-ext-install -j$(nproc) \
	soap \
	sockets \
	calendar \
	sysvmsg \
	sysvsem \
	sysvshm \
	ctype \
	iconv \
	fileinfo \
	xmlwriter

# Install form source

# ext-apcu
ENV EXT_APCU_VERSION=5.1.17

RUN mkdir -p /usr/src/php/ext/apcu \
	&& curl -fsSL https://github.com/krakjoe/apcu/archive/v$EXT_APCU_VERSION.tar.gz | tar xvz -C /usr/src/php/ext/apcu --strip 1 \
	&& docker-php-ext-install apcu \
	&& echo "[apcu]" >> /usr/local/etc/php/conf.d/docker-php-ext-apcu.ini \
	&& echo "apc.enabled=1" >> /usr/local/etc/php/conf.d/docker-php-ext-apcu.ini

# ext-opache
RUN docker-php-source extract \
	&& docker-php-ext-enable opcache \
	&& echo "zend_extension=opcache" >> /tmp/docker-php-ext-opcache.ini \
	&& echo "opcache.enable=1" >> /tmp/docker-php-ext-opcache.ini \
	&& echo "opcache.enable_cli=0" >> /tmp/docker-php-ext-opcache.ini \
	&& echo "opcache.memory_consumption=356" >> /tmp/docker-php-ext-opcache.ini \
	&& echo "opcache.max_accelerated_files=100000" >> /tmp/docker-php-ext-opcache.ini \
	&& echo "opcache.validate_timestamps=0" >> /tmp/docker-php-ext-opcache.ini

# x-debug
RUN pecl install xdebug-3.1.2 \
	&& docker-php-ext-enable xdebug \
	&& echo "xdebug.remote_autostart=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
	&& echo "xdebug.remote_enable=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# clean up
RUN docker-php-source delete

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

RUN cd /usr/local/etc/php/conf.d/ && \
	echo 'memory_limit = 2G' >> /usr/local/etc/php/conf.d/docker-php-memlimit.ini

# Add user
RUN groupadd -g 1000 app \
    && useradd -g 1000 -u 1000 -d /var/www -s /bin/bash app

RUN mkdir -p /var/www/html \
    && chown -R app:app /var/www

USER app:app
VOLUME /var/www
WORKDIR /var/www/html
