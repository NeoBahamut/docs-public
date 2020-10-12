# commandes docker

## obtenir de l'aide

```bash
docker
```

usage :  `docker [OPTIONS] COMMAND`

plus d'info sur une commande : `docker COMMAND --help`

Management Commands: `builder, config, container, context, image, network, node, plugin, secret, service,  stack, swarm, system, trust, volume`

Commands: `attach, build, commit, cp, create, diff, events, exec, export, history, images, import, info, inspect, kill, load, login, logout, logs, pause, port, ps, pull, push, rename, restart, rm, rmi, run, save, search, start, stats, stop, tag, top, unpause, update, version, wait`

## tirer une image (ou un repo)

```bash
docker pull `une_image`
```

pointe vers le DockerHub par défaut

l'image la plus light étant alpine : `docker pull alpine`

## créer une image

```bash
docker commit -m "création d'une image" `n°_conteneur` `mon_image:v1.0`
```

## obtenir des infos sur une image

```bash
docker history `n°_image`
```

## lancer un conteneur

### avec un nom et un hostname

```bash
docker run -d --name nom_conteneur --hostname mon_hostname alpine:latest
```

### avec un port partagé, et un volume

```bash
docker run -d -p 8088:80 -v /srv/data/local_nginx:/usr/share/nginx/html nginx:latest
```

### avec un volume monté

```bash
docker run -d -p 8088:80 --mount source=mon_volume,target=/usr/share/nginx/html nginx:latest
```

### avec une variable d'environnement

```bash
docker run -d --env MYVARIABLE="123456" ubuntu:latest
```

### avec un fichier .env

```bash
docker run -d --env-file .env ubuntu:latest
```

### avec un réseau

```bash
docker run -d --network `mon_reseau` ubuntu:latest
```

### un matomo lié à un mysql comme bdd

```bash
docker run -d -p 8088:80  --link mon_mysql:db -v mon_matomo:/var/www/html matomo
```

## lister les infos du conteneur

docker inspect `mon_container`

## lister les images

```bash
docker image ls
```

## lister les containers actifs en local

```bash
docker ps
```

## lister tous les containers en local

```bash
docker ps -a
```

## stopper un container

copier/coller un id, puis

```bash
docker stop `id`
```

## supprimer un container

```bash
docker rm [-rf] `mon_container`
```

## entrer dans un conteneur

### en bash

```bash
docker exec -it mon_matomo `bash`
```

### en psql

```bash
docker exec -it mon_conteneur_bdd `psql`
```

## lancer une commande dans le conteneur et récupérer le résultat en local (ex : un dump)

```bash
docker exec mon_mysql sh -c 'exec mysqldump -pmdp matomo' > matomo.sql
```

## builder un conteneur

```bash
docker build -t ocr-docker-build .
```

## inspecter un conteneur

```bash
docker logs <nom_conteneur>
```

## `docker volume`

  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes

## créer un volume

```bash
docker volume create `mon_volume`
```

## DockerFile

Fichier de config pour création d'image

Séquence d'instructions :

* RUN : lancement de commandes
* ENV : variables d'environnement
* EXPOSE : exposition de ports
* VOLUME : définition de volumes
* COPY : copie entre host et conteneur
* ENTRYPOINT : processus maître

## Créer une image à partir du DockerFile

```bash
docker build -t `mon_image:v1.0` .
```

le . indique le répertoire

## Réseau : 172.17.0.0 par défaut

### Créer un réseau

```bash
docker network create `mon_reseau`
docker network create -d bridge `mon_reseau`
docker network create -d bridge --subnet 172.30.0.16 `mon_reseau`
```
