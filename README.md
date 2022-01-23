# Docker Magento

Docker magento support

* php:7.4.20
* composer:2.1.3
* mysql:8.0
* elasticsearch:7.13.2
* php-myadmin

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

mkdir -p data/backup data/es-data data/mysql-data

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

## Setup magento
```bash
php bin/magento setup:install --base-url=https://magento.local/ \
--db-host=mysql --db-name=magento --db-user=root --db-password=123456 \
--admin-firstname=Dylan --admin-lastname=Ngo --admin-email=it.dylanngo@gmail.com \
--admin-user=admin --admin-password=admin123 --language=vi_VN --currency=VND --timezone=Asia/Ho_Chi_Minh \
--session-save=db --use-rewrites=1 --use-secure=1 --use-secure-admin=1 --elasticsearch-host=elasticsearch --elasticsearch-port=9200 --search-engine=elasticsearch7 --elasticsearch-index-prefix=pdm --elasticsearch-enable-auth=false  --cleanup-database
```

## Config nginx domain/ssl
```
cp conf/nginx/default.conf.sample conf/nginx/default.conf

```

## Edit hosts file in local workstation
```
vim /etc/hosts

Add line

127.0.0.1 magento.local
```

## Move database to backup folder, It'll help you to import database.

## XDebug config with PhpStorm

![Step 1](./data/backup/debug1.png)
![Step 2](./data/backup/debug2.png)
![Step 3](./data/backup/debug3.png)

## OPCache Toolkit
```bash

docker exec -ti --user root php bash

# Clean OPCache
opcache

Enter: clean

# Enable OPCache
opcache

Enter: on

# Disable OPCache

Enter: off
```

## XDebug Toolkit
```bash

docker exec -ti --user root php bash

xdebug -h

# Enable XDebug
xdebug

Enter: on

# Disable XDebug
xdebug

Enter: off
```

Go to mysqladmin: http://localhost:8080
