# Nginx
### Run and go Docker setup for Nginx reverse proxy with autodiscovery and Let's Encrypt.

<br />

To start Nginx on background via Docker just type:
```
docker compose up -d
```

Main parametrs can be modified via `.env` file.


Default Nginx configuration:
```
IMAGE: nginxproxy/nginx-proxy:latest
CONTAINER NAME: nginx-proxy
PORT: 80:80
      443:443
```

Default Let's Encrypt configuration:
```
IMAGE: nginxproxy/acme-companion:latest
CONTAINER NAME: nginx-proxy-acme
```

Data are persisted locally:
```
./volumes/acme
./volumes/certs
./volumes/html
./volumes/vhost.d
```

Docker network:
```
nginx-proxy
```

To reload nginx configuration just run:
```
./reload-nginx.sh
```

Example of Docker Compose configuration for App service, e.g. Adminer:
```
version: '3.8'

services:
    adminer:
        environment:
            VIRTUAL_HOST: ${VIRTUAL_HOST}
            LETSENCRYPT_HOST: ${LETSENCRYPT_HOST}
            LETSENCRYPT_EMAIL: ${LETSENCRYPT_EMAIL}
        networks:
            - nginx-proxy

networks:
    nginx-proxy:
        external: true
```