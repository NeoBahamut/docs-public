# commandes docker-compose

## récupèrer l’ensemble des images décrites dans docker-compose.yml et/ou télécharger depuis le docker Hub (maj)

```bash
docker-compose pull
```

## lancer l’ensemble des conteneurs (le stack) décrits dans docker-compose.yml

se placer à la racine du projet

### à partir du fichier `docker-compose.yml` à la racine

```bash
docker-compose up [-d]
```

`-d` mode détaché

### à partir d'un autre fichier

```bash
docker-compose -f ./mon-fichier.yml up [–build]
```

`-f` précise le fichier
`--build` builde les images avant de démarrer les conteneurs
`--no-build` ne construit pas les images

## builder les images uniquement

```bash
docker-compose build
```

## conteneurs en tant que service (dans le docker-compose)

### voir l'état des conteneurs en tant que service

```bash
docker-compose ps
```

### démarrer des conteneurs en tant que service

```bash
docker-compose start
```

### stopper des conteneurs en tant que service

```bash
docker-compose stop
```

### supprimer des conteneurs en tant que service

```bash
docker-compose rm
```

### répliquer des conteneurs en tant que service

```bash
docker-compose scale SERVICE=3
```

### affiche les logs

```bash
docker-compose logs -f –tail 5
```

### détruit l’ensemble des ressources de la stack

```bash
docker-compose down
```

### valide la syntaxe du fichier

```bash
docker-compose config
```

## docker-compose.yml

Première ligne : la version de docker-compose

```bash
version: '3'
```

Deuxième ligne : les services

```bash
services:
  service1:
    container_name: $NOM_CONTENEUR1
    |
    |
  service2:
    container_name: $NOM_CONTENEUR2
    |
    |
```

[Instructions](https://docs.docker.com/compose/compose-file/) pour chaque service :

* container_name : nom du conteneur
* [image](https://docs.docker.com/compose/compose-file/#image) : image du conteneur | nom de l'image buildée (si option `build`)
* [[restart]](https://docs.docker.com/compose/compose-file/#restart) : politique de redémarrage = (no) | always | on-failure | unless-stopped
* [[entrypoint]](https://docs.docker.com/compose/compose-file/#entrypoint) : surcharge le point d'entrée de l'image
* [[command]](https://docs.docker.com/compose/compose-file/#command) : surcharge la commande par défaut de l'image
* [[build]](https://docs.docker.com/compose/compose-file/#build) : infos sur le build = un path (string) de répertoire (ex : . ) | un objet :
                                                              context : un path (string) de répertoire (ex : . )
                                                              dockerfile : spécifie un autre fichier 'DockerFile'
                                                              args : (=variables d'environnement uniquement pour le process de build)
                                                                numero_de_build: 1
* [[depends_on]](https://docs.docker.com/compose/compose-file/#depends_on) : dépendance d'un autre service
* [[env_file]](https://docs.docker.com/compose/compose-file/#env_file) : spécifie un ou n fichier(s) .env (chemin relatif au fichier spécifié avaec -f)
* [[environment]](https://docs.docker.com/compose/compose-file/#environment) : spécifie des variables d'environement (surcharge `env_file`)
* [[expose]](https://docs.docker.com/compose/compose-file/#expose) : ports exposés aux services liés
* [[networks]](https://docs.docker.com/compose/compose-file/#networks) : réseaux = noms de réseau | nom + alias | nom + IP
