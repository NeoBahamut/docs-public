
# Installer Docker

[Install Docker Engine on Debian](https://docs.docker.com/engine/install/debian/)

## trouver son architecture : s'assurer que l'archi est dans la liste de la doc

```bash
    uname -m
```

> x86_64

## installation

### Install using the repository

### Set up the repository

1. Update the apt package index and install packages to allow apt to use a repository over HTTPS

2. Add Docker’s official GPG key

    Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint

3. Use the following command to set up the stable repository. To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below

choix de l'architecture : x86_64 / amd64

### Install Docker Engine

1. Update the apt package index, and install the latest version of Docker Engine and containerd, or go to the next step to install a specific version

2. To install a specific version of Docker Engine (passer cette étape)

3. Verify that Docker Engine is installed correctly by running the hello-world image

## Installer docker-compose

[https://docs.docker.com/compose/install/]

### Install Compose on Linux systems

1. Run this command to download the current stable release of Docker Compose

2. Apply executable permissions to the binary

## Créer le groupe `docker` et y ajouter l'utilisateur

```bash
    sudo groupadd docker

    sudo gpasswd -a $USER docker

    newgrp docker
```
