FROM php:7.4-fpm

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=1.10.16

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
		telnet

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
		xmlrpc \
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

# hash
# RUN apt-get install -y \
# 	libmhash2 \
#     libmhash-dev \
#     libc-client-dev \
#     libkrb5-dev \
#     libssh2-1-dev \
#     libmcrypt-dev \
#     && docker-php-ext-configure hash --with-mhash \
#     && docker-php-ext-install -j$(nproc) hash

# openssl
# RUN docker-php-ext-install -j$(nproc) openssl

# others
RUN docker-php-ext-install -j$(nproc) \
	soap \
	sockets \
	calendar \
	sysvmsg \
	sysvsem \
	sysvshm \
    ctype \
    iconv

# RUN pecl install xdebug-3.0.2 \
#     && docker-php-ext-enable xdebug \
#     && echo "xdebug.remote_enable=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#     && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#     && echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#     && echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#     && echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#     && echo "xdebug.idekey=PHPSTORM" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

#RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
RUN cd /usr/local/etc/php/conf.d/ && \
  echo 'memory_limit = -1' >> /usr/local/etc/php/conf.d/docker-php-memlimit.ini
# RUN curl -Ls https://raw.githubusercontent.com/newrelic/newrelic-cli/master/scripts/install.sh | bash && NEW_RELIC_API_KEY=NRAK-JVXF2L7SUF78MQW0O27LTTMMHP1 NEW_RELIC_ACCOUNT_ID=3195151 /usr/local/bin/newrelic install
RUN groupadd -g 1000 app \
    && useradd -g 1000 -u 1000 -d /var/www -s /bin/bash app
RUN mkdir -p /var/www/html \
    && chown -R app:app /var/www

USER app:app
VOLUME /var/www
WORKDIR /var/www/html
