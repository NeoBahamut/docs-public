# Installer git

## Installer git via apt

```bash
sudo apt install git
```

## créer un user git

```bash
sudo adduser git
```

### créer le dir .ssh de git avec les droits

```bash
sudo mkdir -p /home/git/.ssh
sudo chmod -R 700 /home/git/.ssh
```

### créer le fichier authorized_keys

```bash
sudo nano /home/git/.ssh/authorized_keys
```

### rendre l'utilisateur git owner de son home

```bash
sudo chown -R git:git /home/git
```

### rendre l'utilisateur git owner de /srv

```bash
sudo chown -R git:users /srv
```

### ajouter git dans le groupe docker

```bash
sudo usermod -a -G docker git
```

## ajouter des clés SSH à l'utilisateur git

### Copier la clé ssh du disque Windows dans le fichier authorized_keys

Pour copier la première clé :

```bash
scp c:/.../id_rsa.pub git@$IP:~/.ssh/authorized_keys
```

Pour ajouter des clés supplémentaires : éditer le fichier /home/git/.ssh/authorized_keys et y ajouter les clés à la ligne