# Partie : Files and users

# I. Fichiers

## 1. Find me

- Dans un premier temps je me connecte à ma VM en changeant son IP pour la mettre sur le même réseau puis avec la commande `$ ssh djo@192.168.56.2` je me connecte en ssh.

🌞 **Trouver le chemin vers le répertoire personnel de votre utilisateur**

![utilisateur](../main/img/utilisateur.png)


🌞 **Trouver le chemin du fichier de logs SSH**

grâce à la commande `cd /var/log` et `sudo nano secure`

![log ssh](../main/img/log%20ssh.png)

🌞 **Trouver le chemin du fichier de configuration du serveur SSH**

- Le chemin est `cd etc/ssh/sshd_config.d`
  on peut le voir avec un `ls -a` dans ssh
  ![config ssh](../main/img/dossier_config_ssh.png)

# II. Users

## 1. Nouveau user

🌞 **Créer un nouvel utilisateur**

- Pour réaliser ceci je me suis servi de la doc [Rocky Linux](https://docs.rockylinux.org/books/admin_guide/06-users/)

- Commandes pour créer l'utilisateur: 
  - `$ sudo useradd -u 1001 -d /home/papier_alu/ marmotte`
- Puis création du mot de passe et déverouillage du compte:
  - `$ sudo usermod -p chocolat marmotte`
  - `$ sudo usermod -U marmotte`

## 2. Infos enregistrées par le système

🌞 **Prouver que cet utilisateur a été créé**

![config ssh](../main/img/user_marmotte.png)


🌞 **Déterminer le *hash* du password de l'utilisateur `marmotte`**

- Je suis allé dans `/etc/shadow` mais le mdp était en clair.
![shadow](../main/img/shadow_marmotte.png)
- J'ai donc vérifié si le `ENCRYPT_METHOD` était défini mais ça avait l'air good.
![SHA512](../main/img/SHA512.png)
Donc je sais pas pourquoi et j'ai pas creusé plus mais je veux bien une explication.


## 3. Connexion sur le nouvel utilisateur

🌞 **Tapez une commande pour vous déconnecter : fermer votre session utilisateur**

- J'ai utilisé `logout` ça m'a déci du SSH.
- Je me suis reco sur `SSH marmotte@<IP>` mais le mdp ne fonctionnait pas.
- Du coup je suis passé par `sudo su - marmotte` je sais pas si c'est de la triche ? 

🌞 **Assurez-vous que vous pouvez vous connecter en tant que l'utilisateur `marmotte`**

![triche?](../main/img/marmotte.png)

# Partie 2 : Programmes et paquets

# I. Programmes et processus


## 1. Run then kill

🌞 **Lancer un processus `sleep`**

![sleep](../main/img/sleep.png)

![grep_sleep](../main/img/grep_sleep.png)


🌞 **Terminez le processus `sleep` depuis le deuxième terminal**

![kill](../main/img/kill_sleep.png)

## 2. Tâche de fond

🌞 **Lancer un nouveau processus `sleep`, mais en tâche de fond**

- J'effectue la même commande en rajoutant `&`

🌞 **Visualisez la commande en tâche de fond**

![jobs](../main/img/jobs.png)

## 3. Find paths


🌞 **Trouver le chemin où est stocké le programme `sleep`**

![ls_sleep](../main/img/ls_sleep.png)

🌞 Tant qu'on est à chercher des chemins : **trouver les chemins vers tous les fichiers qui s'appellent `.bashrc`**

![find](../main/img/find.png)

## 4. La variable PATH

🌞 **Vérifier que**
- les commandes `sleep`, `ssh`, et `ping` sont bien des programmes stockés dans l'un des dossiers listés dans votre `PATH`

![verif](../main/img/verif.png)


# II. Paquets

➜ **Tous les OS Linux sont munis d'un store d'application**

🌞 **Installer le paquet `git`**

![dnf_git](../main/img/dnf_git.png)
![complete](../main/img/complete.png)

🌞 **Utiliser une commande pour lancer Firefox**

![git](../main/img/git.png)

🌞 **Installer le paquet `nginx`**

![nginx](../main/img/nginx.png)

🌞 **Déterminer**

- le chemin vers le dossier de logs de NGINX souligné en rouge 
- le chemin vers le dossier qui contient la configuration de NGINX souligné en vert

![log](../main/img/log.png)

🌞 **Mais aussi déterminer...**

- Le dossier qui contient les URL est `yum.repos.d`

# Partie 3 : Poupée russe

Pour finir de vous exercer avec le terminal, je vous ai préparé une poupée russe :D



➜ **Le but de l'exercice pour vous :**

🌞 **Récupérer le fichier `meow`**

- J'installe `wget` avec `sudo dnf install wget`
- Je dl le fichier avec `sudo wget URL`

🌞 **Trouver le dossier `dawa/`**

- Je trouve le chemin avec `find / -name`
- Je trouve le type de fichier avec `file /path/`

![meow](../main/img/meow.png)

![rename](../main/img/rename.png)

- J'installe `zip` et je décompresse
- Après avoir check avec `file` le fichier est compressé avec XZ. Je décompresse avec `unxz <file>`.
- Re check et le `meow` est `RAR archive data, v5`
- J'ai pas réussi à installer `rar` avec `sudo dnf install rar unrar` du coup je suis allé sur rarlab.com et j'ai rentré la suite de commandes:
```
$ cd /tmp
$ wget https://www.rarlab.com/rar/rarlinux-x64-700b2.tar.gz
$ tar -zxvf rarlinux-x64-700b2.tar.gz
$ cd rar
$ sudo cp -v rar unrar /usr/local/bin/
```
- et là après check je vois `gzip compressed data, from Unix, original size modulo 2^32 145049600`
- J'installe `tar` je rename `meow` en `meox.tar.gz`
- Je décompresse avec `tar -xvzf <file>` et là enfin j'arrive sur `dawa`

🌞 **Dans le dossier `dawa/`, déterminer le chemin vers**

- Après le calcule de 15 Mo en kilooctets j'utilise `find`
  ![15](../main/img/15.png)

- Pour trouver le fichier qui contient que des 7 j'ai galéré et j'ai pas réussi à en trouver qu'un, après plusieurs tentatives j'en ai 2. 
  ![7](../main/img/7.png)
- Pour `cookie` beaucoup plus simple
  ![cookie](../main/img/cookie.png)
- Fichier caché identique sauf qu'il faut pas oublier `*` pour préciser que le fichier commence par un `.`
  ![hidden](../main/img/hidden.png)

- le seul fichier qui date de 2014
- le seul fichier qui a 5 dossiers-parents
  - je pense que vous avez vu que la structure c'est 50 `folderX`, chacun contient 50 dossiers `X`, et chacun contient 50 `fileX`
  - bon bah là y'a un fichier qui est contenu dans `folderX/X/X/X/X/` et c'est le seul qui 5 dossiers parents comme ça

![Matryoshka](./img/dolls.png)



## Très bon TP, j'ai beaucoup appris ! 
Juste quelques interrogatons sur le hash et le mdp non reconnu de marmotte.