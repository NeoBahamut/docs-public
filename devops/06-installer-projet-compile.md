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

## ajouter les droits d'exécution

```bash
sudo chmod +x /srv/scripts/project-create
sudo chmod +x /srv/scripts/project-delete
```

## créer le répertoire du projet $PROJET

```bash
/srv/scripts/project-create $PROJET
```

## Variables d'environnement : alimenter le fichier .env

```bash
sudo nano /srv/env/$PROJET/.env
```

## ajout de la remote ${PROJET}-deploy : en local

```bash
git remote add ${PROJET}-deploy ssh://git@$IP/srv/git/$PROJET.git
```

## (re)rendre l'utilisateur git owner de /srv

```bash
sudo chown -R git:users /srv
```

## configurer le hook

```bash
sudo nano /srv/git/$PROJET.git/hooks/post-receive
```

s'assurer de lancer docker-compose avc le fichier ad hoc, par exemple, en fin de fichier :

```bash
docker-compose -f $DOCKER_COMPOSE_FILE up -d
```

## déploiement sur la remote : en local

```bash
git push ${PROJET}-deploy master
```
