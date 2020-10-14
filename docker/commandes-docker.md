# commandes docker

## obtenir de l'aide

```bash
docker
```

usage :  `docker [OPTIONS] COMMAND`

plus d'info sur une commande : `docker COMMAND --help`

Management Commands: `builder, config, container, context, image, network, node, plugin, secret, service,  stack, swarm, system, trust, volume`

Commands: `attach, build, commit, cp, create, diff, events, exec, export, history, images, import, info, inspect, kill, load, login, logout, logs, pause, port, ps, pull, push, rename, restart, rm, rmi, run, save, search, start, stats, stop, tag, top, unpause, update, version, wait`

## Image

### tirer une image (ou un repo)

```bash
docker pull `une_image`
```

pointe vers le DockerHub par défaut

l'image la plus light étant alpine : `docker pull alpine`

### créer une image

```bash
docker commit -m "création d'une image" `n°_conteneur` `mon_image:v1.0`
```

### obtenir des infos sur une image

```bash
docker history `n°_image`
```

### lister les images

```bash
docker image ls
```

ou

```bash
docker images
```

### Créer une image à partir du [DockerFile](https://github.com/NeoBahamut/docs-public/blob/master/docker/commandes-docker.md#dockerfile)

```bash
docker build -t `mon_image:v1.0` .
```

le . indique le répertoire

### Créer une image à partir d'un conteneur

```bash
docker commit -m "commentaire du commit" `id_conteneur` `mon_image:v1.0` .
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

### avec le volume d'un autre conteneur

```bash
docker run -d --volumes-from `autre_conteneur` nginx:latest
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
docker run -d --network[--net] `mon_reseau` ubuntu:latest
```

### avec un lien vers un autre conteneur : ajout dans /etc/hosts

```bash
docker run -d --link `autre_conteneur` ubuntu:latest
```

### avec ajout d'host (ex: machines physiques)

```bash
docker run -d --add-host `mon_host:122.0.25.3` ubuntu:latest
```

### avec ajout de dns : ajout dans le resolve.conf

```bash
docker run -d --dns `172.0.0.3` ubuntu:latest
```

### un matomo lié à un mysql comme bdd

```bash
docker run -d -p 8088:80  --link mon_mysql:db -v mon_matomo:/var/www/html matomo
```

## Infos/action sur un conteneur

### inspecter un conteneur

```bash
docker logs <nom_conteneur>
```

### lister les infos du conteneur

docker inspect `mon_container`

### lister les containers actifs en local

```bash
docker ps
```

### lister tous les containers en local

```bash
docker ps -a
```

### stopper un container

copier/coller un id, puis

```bash
docker stop `id`
```

### supprimer un container

```bash
docker rm [-flv] `mon_container`
```

`-f` force
`-l` supprime le lien
`-v` supprime le volume associé

## entrer dans un conteneur

### en bash

```bash
docker exec -it mon_matomo `bash`
```

### en psql

```bash
docker exec -it mon_conteneur_bdd `psql`
```

### lancer une commande dans le conteneur et récupérer le résultat en local (ex : un dump)

```bash
docker exec mon_mysql sh -c 'exec mysqldump -pmdp matomo' > matomo.sql
```

## Volume

  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes

### créer un volume

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

## Réseau

bridge (172.17.0.0) = réseau par défaut

### Créer un réseau

```bash
docker network create `mon_reseau`
```

### Créer un réseau : spécifier le type (bridge par défaut)

```bash
docker network create -d[--driver] bridge `mon_reseau`
```

### Créer un réseau : spécifier le sous-réseau

```bash
docker network create -d bridge --subnet 172.30.0.16 `mon_reseau`
```

## Cache

Pour ne pas utiiser le cache, utiliser l'instruction `--no-cache`

### dans le build : pour toutes les instructions

```bash
docker build --no-cache -t `mon_image`
```

### dans le DockerFile : pour une instruction

Utile sur `apt-get update` sinon l'update n'est pas recalculé

```bash
FROM alpine:latest

RUN apt-get update --no-cache

ENV ma_variable = 0
```

Placer en dernier les éléments susceptibles d'être recalculés (comme les variables d'environnement) car dès que le cache n'est pas utilisé, c'est également le cas pour les étapes suivantes
