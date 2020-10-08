# Installation

## copie des script project-create et project-delete sous /srv/scripts

```bash
sudo nano /srv/scripts/project-create
```

source : [https://gist.github.com/francoisromain/58cabf43c2977e48ef0804848dee46c3]

```bash
sudo nano /srv/scripts/project-delete
```

source : [https://gist.github.com/francoisromain/e28069c18ebe8f3244f8e4bf2af6b2cb]

## créer le répertoire du projet $PROJET

```bash
/srv/script/project-create $PROJET
```

## Variables d'environnement : créer le fichier .env et l'alimenter

```bash
sudo nano /srv/env/$PROJET/.env
```

## ajout de la remote ${PROJET}-deploy : en local

```bash
git remote add ${PROJET}-deploy ssh://git@$IP/srv/git/$PROJET.git
```

```bash
sudo chown -R git:users /srv
```

## déploiement sur la remote

```bash
git push ${PROJET}-deploy master
```

## configurer le hook

```bash
sudo nano /srv/git/$PROJET.git/hooks/post-receive
```

### lancer docker-compose

Par exemple, en fin de fichier :

```bash
docker-compose -f $DOCKER_COMPOSE_FILE up -d
```
