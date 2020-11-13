# Configuration d'un VPS Debian

## Configuration initiale

### Se connecter en tant qu'admin en SSH

```bash
ssh $USER@$IP
```

### Update/Upgrade du systeme

```bash
sudo apt-get update && sudo apt-get upgrade
```

### Modifier le hostname

```bash
hostnamectl set-hostname $HOSTNAME

# Update the hosts file
nano /etc/hosts
# In the `hosts` file, add this line after the `127.0.1.1`:
# example:
# 0.0.0.0 my-host
<ip> <hostname>
```

Save and exit Nano: `ctrl` + `X`, `Y`, `enter`.

### Modifier la timezone

#### Consulter

```bash
timedatectl
```

#### Changer

```bash
sudo timedatectl set-timezone Europe/Paris
```

#### Contrôler

```bash
timedatectl
```

## Sécuriser le serveur

### Mises à jour automatiques

```bash
sudo apt-get install unattended-upgrades apt-listchanges
```

#### Editer 50unattended-upgrades

```bash
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```

#### Vérifier 50unattended-upgrades

```bash
//      "origin=Debian,codename=${distro_codename}-updates";
//      "origin=Debian,codename=${distro_codename}-proposed-updates";
        "origin=Debian,codename=${distro_codename},label=Debian";
        "origin=Debian,codename=${distro_codename},label=Debian-Security";
```

#### Editer 20auto-upgrade

```bash
sudo nano /etc/apt/apt.conf.d/20auto-upgrades
```

#### Vérifier 20auto-upgrade

```bash
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

### Ajouter un utilisateur

#### Créer l'utilisateur

```bash
sudo adduser $USER
```

#### Ajouter l'utilisateur au groupe sudo

```bash
sudo adduser $USER sudo
```

### Ajouter la clé ssh de l'utilisateur

#### Se logger en tant qu'utilisateur

```bash
    ssh $USER@$IP
```

#### Créer un répertoire `.ssh` sur le serveur

```bash
# créer le répertoire
mkdir -p ~/.ssh

# définir les permissions
chmod -R 700 ~/.ssh/
```

#### Copier la clé ssh du disque Windows dans le fichier authorized_keys

```bash
scp c:/.../id_rsa.pub $USER@$IP:~/.ssh/authorized_keys
```

#### Vérifier en se connectant en ssh

```bash
ssh $USER@$IP
```

#### Interdire les connexions root en SSH

Sur le serveur, éditer le fichier `/etc/ssh/sshd_config`

```bash
PermitRootLogin no
PasswordAuthentication no
```

#### Redémarrer le service ssh

```bash
sudo /etc/init.d/ssh restart
```

#### Modifier le port d'écoute

#### Éditer sshd_config

```bash
nano /etc/ssh/sshd_config
```

#### Changer le port

```bash
Port $PORT
```

#### Redémarrer le service

```bash
sudo /etc/init.d/ssh restart
```

#### Vérifier

```bash
ssh $USER@$IP -p $PORT
```
