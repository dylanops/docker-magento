# Docker magento 2.4.x

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

* Move database to backup folder, It'll help you to import database.
* For XDebug you should map source on local to source on server
