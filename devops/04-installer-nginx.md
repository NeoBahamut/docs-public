# Installer nginx-proxy

## Documentation source

[https://medium.com/@francoisromain/host-multiple-websites-with-https-inside-docker-containers-on-a-single-server-18467484ab95]

## Créer le répertoire et le fichier docker-compose.yml

```bash
sudo mkdir -p /srv/www/nginx-proxy
sudo nano /srv/www/nginx-proxy/docker-compose.yml
```

### Contenu de docker-compose.yml

```bash
version: '3'
services:
  nginx:
    image: nginx
    labels:
      com.github.jrcs.letsencrypt_nginx_proxy_companion.nginx_proxy: "true"
    container_name: nginx
    restart: unless-stopped
    logging:
      options:
        max-size: "10m"
        max-file: "3"
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /srv/www/nginx-proxy/conf.d:/etc/nginx/conf.d
      - /srv/www/nginx-proxy/vhost.d:/etc/nginx/vhost.d
      - /srv/www/nginx-proxy/html:/usr/share/nginx/html
      - /srv/www/nginx-proxy/certs:/etc/nginx/certs:ro
  nginx-gen:
    image: jwilder/docker-gen
    command: -notify-sighup nginx -watch -wait 5s:30s /etc/docker-gen/templates/nginx.tmpl /etc/nginx/conf.d/default.conf
    container_name: nginx-gen
    restart: unless-stopped
    volumes:
      - /srv/www/nginx-proxy/conf.d:/etc/nginx/conf.d
      - /srv/www/nginx-proxy/vhost.d:/etc/nginx/vhost.d
      - /srv/www/nginx-proxy/html:/usr/share/nginx/html
      - /srv/www/nginx-proxy/certs:/etc/nginx/certs:ro
      - /var/run/docker.sock:/tmp/docker.sock:ro
      - /srv/www/nginx-proxy/nginx.tmpl:/etc/docker-gen/templates/nginx.tmpl:ro
  nginx-letsencrypt:
    image: jrcs/letsencrypt-nginx-proxy-companion
    container_name: nginx-letsencrypt
    restart: unless-stopped
    volumes:
      - /srv/www/nginx-proxy/conf.d:/etc/nginx/conf.d
      - /srv/www/nginx-proxy/vhost.d:/etc/nginx/vhost.d
      - /srv/www/nginx-proxy/html:/usr/share/nginx/html
      - /srv/www/nginx-proxy/certs:/etc/nginx/certs:rw
      - /var/run/docker.sock:/var/run/docker.sock:ro
    environment:
      NGINX_DOCKER_GEN_CONTAINER: "nginx-gen"
      NGINX_PROXY_CONTAINER: "nginx"
networks:
  default:
    external:
      name: nginx-proxy
```

## Créer le fichier nginx.tmpl et y copier la source

```bash
sudo nano /srv/www/nginx-proxy/nginx.tmpl
```

source : [https://raw.githubusercontent.com/jwilder/nginx-proxy/master/nginx.tmpl]

## Créer la structure du projet

```bash
sudo mkdir /srv/www/nginx-proxy/conf.d
sudo mkdir /srv/www/nginx-proxy/vhost.d
sudo mkdir /srv/www/nginx-proxy/html
sudo mkdir /srv/www/nginx-proxy/certs
```

## Créer le réseau

```bash
docker network create nginx-proxy
```

## Démarrer le conteneur

```bash
cd /srv/www/nginx-proxy/
docker-compose up -d
```