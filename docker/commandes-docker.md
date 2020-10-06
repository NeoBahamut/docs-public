# commandes docker

## tirer un conteneur

```bash
docker pull `un_container`
```

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

## lancer un conteneur

```bash
docker run -d -p 8088:80  --link mon_mysql:db -v mon_matomo:/var/www/html matomo
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
