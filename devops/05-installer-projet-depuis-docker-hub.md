# Installation `prod`

## Créer le répertoire et le fichier .env du projet $PROJET

```bash
sudo mkdir -p /srv/www/$PROJET
```

## Variables d'environnement : créer le fichier .env et l'alimenter

```bash
sudo nano /srv/www/$PROJET/.env
```

## Docker Compose : créer le fichier docker-compose.yml et l'alimenter

```bash
sudo nano /srv/www/$PROJET/docker-compose.yml
```

## Créer le répertoire et les fichiers de scripts

```bash
sudo mkdir -p /srv/scripts
```

```bash
sudo nano /srv/scripts/${PROJET}-deploy
```

### contenu du fichier ${PROJET}-deploy

### ajouter les droits d'exécution

```bash
sudo chmod +x /srv/scripts/${PROJET}-deploy
```

## deployer le projet $PROJET

```bash
/srv/scripts/${PROJET}-deploy
```
