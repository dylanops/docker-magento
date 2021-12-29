# Docker Magento

Docker magento support

* php:7.4.20
* composer:1.10.16
* mysql:8.0
* elasticsearch:7.13.2
* cron job

Docker support php libraries

* bcmath
* ctype
* curl
* dom
* gd
* hash
* iconv
* intl
* mbstring
* openssl
* pdo_mysql
* simplexml
* soap
* spl
* xsl
* zip
* libxml
* xdebug 3
* opache
* apcu
* memcached
* curl

# How to use
## Init project
```bash
mv .env.sample .env

Edit config on the .env file
```

## Create self credentials
```bash
cd /conf/nginx
openssl req -newkey rsa:4096 \
            -x509 \
            -sha256 \
            -days 3650 \
            -nodes \
            -out local.crt \
            -keyout local.key
```

## Start/Stop docker
```bash
docker-compose up -d
docker-compose down -v

```

## Move database to backup folder, It'll help you to import database.