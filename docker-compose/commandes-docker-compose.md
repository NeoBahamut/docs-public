# commandes docker-compose

## récupère l’ensemble des images décrites dans docker-compose.yml et/ou télécharge depuis le docker Hub

```bash
docker-compose pull
```

## lance l’ensemble des conteneurs (le stack)  décrits dans docker-compose.yml

se placer à la racine du projet

```bash
docker-compose up [-d]
```

```bash
docker-compose -f ./mon-fichier.yml up –build
```

`-f` précise le fichier
`--build` builde les images avant de démarrer les conteneurs

## stopper un conteneur

```bash
docker-compose stop
```

## killer les conteneurs inactifs

```bash
docker system prune -a
```

## lister les volumes

```bash
docker volume ls
```

## killer tous les volumes inutilisés

```bash
docker volume prune
```

## supprime un volume

```bash
docker volume rm camino-api_data
```

## supprime un container et les volumes

```bash
docker rm -vf camino-api_db
```

## état des conteneurs

se placer à la racine du projet

```bash
docker-compose ps
```

## affiche les logs

```bash
docker-compose logs -f –tail 5
```

## arrête la stack, sans supprimer les différentes ressources créées

```bash
docker-compose stop
```

## détruit l’ensemble des ressources de la stack

```bash
docker-compose down
```

## valide la syntaxe du fichier

```bash
docker-compose config
```
