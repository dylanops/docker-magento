# Docker magento 2.4.x
Docker support libraries

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
* acpu

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

## Start docker
```bash
docker-compose up -d
```

## Stop docker
```bash
docker-compose down -v
```

## Move database to backup folder, It'll help you to import database.

## XDebug config with PhpStorm

![Step 1](./backup/debug1.png)
![Step 2](./backup/debug2.png)
![Step 3](./backup/debug3.png)

Go to mysqladmin: http://localhost:8080