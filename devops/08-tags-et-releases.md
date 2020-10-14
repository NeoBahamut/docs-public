# Effectuer des tags et des releases avec `standard-version` et `conventional-github-releaser`

## Préambule

Déclarer un numéro de version inital dans le fichier `package.json`

```bash
"version": "0.1.1",
```

Respecter les [Conventional Commits Specification](https://conventionalcommits.org)

Le message de commit doit être structuré selon ce modèle :

```bash
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Fonctionnement

### La commande `standard-version` va :

* générer le `CHANGELOG.md` basé sur les différents commits,

* créer un nouveau commit incluant la version et le changelog mis à jour,

* créer un nouveau `tag` avec la nouvelle version.

```bash
npx standard-version
```

### La commande suivante pousse la branche master sur upstream incluant les tags

```bash
git push --follow-tags upstream master
```

### La commmande `conventional-github-releaser` va générer une realease sur GitHub basée sur les derniers commits 

* La présence d'un tag est nécessaire pour effectuer un release

* Générer un nouveau [token](https://github.com/settings/tokens/new) sur GitHub et le copier (le token est introuvable après)

```bash
CONVENTIONAL_GITHUB_RELEASER_TOKEN=<MY_TOKEN> npx conventional-github-releaser
```

* Exporter le token dans le Path 

```bash
export CONVENTIONAL_GITHUB_RELEASER_TOKEN=<MY_TOKEN>
```

la commande est simplifiée

```bash
npx conventional-github-releaser
```
