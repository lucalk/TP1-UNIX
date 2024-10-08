# TP1-UNIX

## Sources 
- Dictionnaire des commandes
- ChatGPT


## 2


### 2.1 
#### Configurer le proxy de la vm
- nano /etc/apt/apt.conf
- Acquire::http::Proxy “http://proxy.ufr-info-p6.jussieu.fr:3128”;

#### Configurer ssh 
- sudo apt update
- apt install openssh-server

#### Connexion root a distance
- Accéder au fichier : nano /etc/ssh/sshd_config
- Modifier : #PermitRootLogin prohibit-password => PermitRootLogin yes
- Sauvegarder

#### Restart le serveur : 
- sudo systemctl restart ssh
- Tester la connexion : ssh root@10.20.0.142

### 2.3
```bash
root@serveur-correction:~# dpkg -l | wc -l
330
```
La machine possède 330 paquets.



### 2.4
```bash
root@serveur-correction:~# df -h
```
| Système de fichiers | Taille | Utilisé | Dispo | Uti% | Monté sur     |
|---------------------|--------|---------|-------|------|---------------|
| udev                |  4,9G  |    0    |  4,9G |  0%  | /dev          |
| tmpfs               | 995M   |  580K   | 994M  |  1%  | /run          |
| /dev/sda1           |  9,1G  |  1,6G   |  7,0G | 19%  | /             |
| tmpfs               |  4,9G  |    0    |  4,9G |  0%  | /dev/shm      |
| tmpfs               |  5,0M  |    0    |  5,0M |  0%  | /run/lock     |
| /dev/sda3           | 921M   |   23M   | 835M  |  3%  | /var/log      |
| /dev/sda2           |  3,6G  |   40K   |  3,4G |  1%  | /tmp          |
| tmpfs               | 995M   |    0    | 995M  |  0%  | /run/user/0   |

La racine représente 1.6G d'espace utilisé.

### 2.5

- Locales : echo $LANG : 
    ```bash
    root@serveur-correction:~# echo $LANG  
    fr_FR.UTF-8
    ```
  Cette commande affiche la valeur de $LANG. La langue utilisé par le système.
  
- Nom machine : hostname :
    ```bash
    root@serveur-correction:~# hostname
    serveur-correction
    ```
  La commande affiche le nom d'hôte de la machine utilisé
  
- Domaine : man hostname :
  ```bash
    NAME
       hostname - show or set the system's host name
       domainname - show or set the system's NIS/YP domain name
  ```
    Résultat
  ```bash
      root@serveur-correction:~# domainname
      (none)
  ```
  
- Vérification emplacement depots : cat /etc/apt/sources.list | grep -v -E '^#|^$' :
  ```bash
      root@serveur-correction:~# cat /etc/apt/sources.list | grep -v -E '^#|^$'
      deb http://deb.debian.org/debian/ bookworm main contrib non-free non-free-firmware
      deb http://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
      deb http://deb.debian.org/debian/ bookworm-updates main contrib non-free non-free-firmware
  ```
  Permet de lister les dépots des paquets configurés dans le fichier
  
- passwd/shadow :
  ```bash
    root@serveur-correction:~# cat /etc/shadow |grep -vE ':\*:|:!\*:'
    root:$y$j9T$cGKpM0Fh8WZYM4MlsbdWz0$hO26Ub/deBLYO3CleXEevZ8v/V.ItKMLsZ274x5BMtA:19635:0:99999:7:::
    messagebus:!:19635::::::
    sshd:!:19998::::::
  ```
  Permet d'afficher le contenu du fichier tout en filtrant certaines lignes avec ses critères
  Affiche les comptes qui ne sont ni désactivés ni vérouillés dans ce fichier.
  
- Compte utilisateurs :
 ```bash
    root@serveur-correction:~# cat /etc/passwd | grep -vE 'nologin|sync'
    root:x:0:0:root:/root:/bin/bash
  ```
  Permet d'afficher les utilisateurs dans le fichier en excluant ceux qui ont les shells nologin ou sync.
  
- Expliquer le retour des commandes :
    -- fdisk -l :
  ```bash
  Modèle de disque : HARDDISK        
  Unités : secteur de 1 × 512 = 512 octets
  Taille de secteur (logique / physique) : 512 octets / 512 octets
  taille d'E/S (minimale / optimale) : 512 octets / 512 octets
  Type d'étiquette de disque : gpt
  Identifiant de disque : 3ED2EBFD-8995-4690-8EEF-8C65108F2339

  | Périphérique |  Début  |     Fin    | Secteurs  | Taille |           Type            |
  |--------------|---------|------------|-----------|--------|---------------------------|
  | /dev/sda1    |   2048  | 19531775   | 19529728  |  9,3G  | Système de fichiers Linux  |
  | /dev/sda2    | 19531776| 27344895   |  7813120  |  3,7G  | Système de fichiers Linux  |
  | /dev/sda3    | 27344896| 29298687   |  1953792  |  954M  | Système de fichiers Linux  |
  | /dev/sda4    | 29298688| 41940991   | 12642304  |   6G   | Partition d'échange Linux  |
  ```
  Permet de lister toutes les partitions de disque disponible sur le système avec plusieurs infos ssur chaque disque.
  Donc le disque, le nombre de cylindre, tête secteurs.
  Et partion avec le type, la taille en blocs et si la partition est bootable (*)
  Affiche les tables de partitions des périphériques indiqués
  
    fdisk -x :
  ```bash
    root@serveur-correction:~# fdisk -x
    Disque /dev/sda : 20 GiB, 21474836480 octets, 41943040 secteurs
    Modèle de disque : HARDDISK        
    Unités : secteur de 1 × 512 = 512 octets
    Taille de secteur (logique / physique) : 512 octets / 512 octets
    taille d'E/S (minimale / optimale) : 512 octets / 512 octets
    Type d'étiquette de disque : gpt
    Identifiant de disque: 3ED2EBFD-8995-4690-8EEF-8C65108F2339
    Premier LBA utilisable: 34
    Dernier LBA utilisable: 41943006
    LBA alternatif: 41943039
    LBA de départ des entrées de partition: 2
    Entrées de partitions allouées: 128
    LBA de fin des entrées de partition: 33

    | Périphérique |  Début  |     Fin    | Secteurs  |                Type-UUID                |                  UUID                  |       Nom        |
    |--------------|---------|------------|-----------|-----------------------------------------|----------------------------------------|------------------|
    | /dev/sda1    |   2048  | 19531775   | 19529728  | 0FC63DAF-8483-4772-8E79-3D69D8477DE4    | 7202EEDF-B999-47CC-BE80-70C80C2C83B0   | la racine        |
    | /dev/sda2    | 19531776| 27344895   |  7813120  | 0FC63DAF-8483-4772-8E79-3D69D8477DE4    | 346085A8-A5AE-48BA-B6B7-BDA598DD7465   | espace tempo     |
    | /dev/sda3    | 27344896| 29298687   |  1953792  | 0FC63DAF-8483-4772-8E79-3D69D8477DE4    | 8F880167-DE17-4896-BB95-EE5AAC9E2E9A   | les logs         |
    | /dev/sda4    | 29298688| 41940991   | 12642304  | 0657FD6D-A4AB-43C4-84E5-0933C84B4F4F    | 68C18C76-59A6-45DF-A745-F87FD1D412DA   | ma swap          |
  ```
Cette commande est comme fdisk -l mais avec plus de détails 

- Retour de commande : df -h :
  ```bash
    root@serveur-correction:~# df -h
    Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
    udev               4,9G       0  4,9G   0% /dev
    tmpfs              995M    580K  994M   1% /run
    /dev/sda1          9,1G    1,6G  7,0G  19% /
    tmpfs              4,9G       0  4,9G   0% /dev/shm
    tmpfs              5,0M       0  5,0M   0% /run/lock
    /dev/sda3          921M     23M  835M   3% /var/log
    /dev/sda2          3,6G     40K  3,4G   1% /tmp
    tmpfs              995M       0  995M   0% /run/user/0
  ```
  Permet d'aficher les tailles en puissance de 2014

  ## 3

### 3.1
  - preseed : Sert à automatiser l'installation des sytèmes d'éxploitation Debian et Ubuntu.
  Il va aussi contenir les réponses des questions posées lors de l'installation.

### 3.2
- Il faut premièrement accéder au GRUB lors du démarrage de la machine.
- Sur une ligne qui commence par linux, il faut faire une modification. Modifier ro : lecture seule par rw : lecture ecriture puis mettre init=/bin/bash. Cela permettra de démarrer en mode root.
- Exécuter la commande : passwd permettant de changer le mot de passe et redémarrer la machine. 

   
              



  







