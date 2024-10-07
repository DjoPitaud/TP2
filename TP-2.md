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

➜ La commande `sleep`, comme toutes les commandes, c'est un programme

- sous Linux, on met pas l'extension `.exe`, s'il y a pas d'extensions, c'est que c'est un exécutable généralement

🌞 **Trouver le chemin où est stocké le programme `sleep`**

- je veux voir un `ls -al /chemin | grep sleep` dans le rendu

🌞 Tant qu'on est à chercher des chemins : **trouver les chemins vers tous les fichiers qui s'appellent `.bashrc`**

- utilisez la commande `find`
- `find` s'utilise comme suit : `find CHEMIN -name NAME`
  - `CHEMIN` c'est un chemin vers un dossier : `find` va chercher des fichiers qui sont contenus dans ce dossier
  - `NAME` est le nom du fichier qu'on cherche

➜ `find` est une commande de ouf qui permet de trouver des fichiers ou dossiers selon plein de critères

```bash
# quelques exemples d'utilisation de find

# trouver tous les .jpg sur tout le disque dur
$ sudo find / -name "*.jpg"

# trouver tous les .jpg sur tout le disque dur, s'ils font + de 10Mo
$ sudo find / -name "./jpg" -size +10M

# c'est un tout ptit aperçu, un peut chercher en fonction de plein plein de critères, et c'est hyper rapide
```

## 4. La variable PATH

**Sur tous les OS (MacOS, Windows, Linux, et autres) il existe une variable `PATH` qui est définie quand l'OS démarre.** Elle est accessible par toutes les applications qui seront lancées sur cette machine.

**On dit que `PATH` est une variable d'environnement.** C'est une variable définie par l'OS, et accessible par tous les programmes.

> *Il existe plein de variables d'environnement définie dès que l'OS démarre, `PATH` n'est pas la seule. On peut aussi en créer autant qu'on veut. Attention, suivant avec quel utilisateur on se connecte à une machine, les variables peuvent être différentes : parfait pour avoir chacun sa configuration !*

**`PATH` contient la liste de tous les dossiers qui contiennent des commandes/programmes.**

Ainsi quand on tape une commande comme `ls /home`, il se passe les choses suivantes :

- votre terminal consulte la valeur de la variable `PATH`
- parmi tous les dossiers listés dans cette variable, il cherche s'il y en a un qui contient un programme nommé `ls`
- si oui, il l'exécute
- sinon : `command not found`

Démonstration :

```bash
# on peut afficher la valeur de la variable PATH à n'importe quel moment dans un terminal
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/bin:/bin
# ça contient bien une liste de dossiers, séparés par le caractère ":"

# si la commande ls fonctionne c'est forcément qu'elle se trouve dans l'un de ces dossiers
# on peut savoir lequel avec la commande which qui interroge aussi la variable PATH
$ which ls
/usr/bin/ls
```

🌞 **Vérifier que**

- les commandes `sleep`, `ssh`, et `ping` sont bien des programmes stockés dans l'un des dossiers listés dans votre `PATH`

# II. Paquets

➜ **Tous les OS Linux sont munis d'un store d'application**

- c'est natif
- quand tu fais `apt install` ou `dnf install`, ce genre de commandes, t'utilises ce store
- on dit que `apt` et `dnf` sont des gestionnaires de paquets
- ça permet aux utilisateurs de télécharger des nouveaux programmes (ou d'autres trucs) depuis un endroit safe

🌞 **Installer le paquet `firefox`**

- c'est uste pour faire pratiquer
- si vous avez choisi un OS sans interface graphique, inutile de télécharger Firefox
  - sans interface graphique, vous pouvez installer le paquet `git` pour remplacer

🌞 **Utiliser une commande pour lancer Firefox**

- comme d'hab, une commande, c'est un programme hein
- déterminer le chemin vers le programme `firefox`
- sans interface graphique, même exercice avec `git` : trouvez le chemin où est stocké la commande

🌞 **Installer le paquet `nginx`**

- il faut utiliser le gestionnaire de paquet natif à l'OS que tu as choisi
- si c'est un système...
  - basé sur Debian, comme Debian lui-même, ou Ubuntu, ou Kali, ou d'autres, c'est `apt` qui est fourni
  - basé sur RedHat, comme Rocky, Fedora, ou autres, c'est `dnf` qui est fourni

🌞 **Déterminer**

- le chemin vers le dossier de logs de NGINX
- le chemin vers le dossier qui contient la configuration de NGINX

🌞 **Mais aussi déterminer...**

- l'adresse `http` ou `https` des serveurs où vous téléchargez des paquets
- une commande `apt install` ou `dnf install` permet juste de faire un téléchargement HTTP
- ma question c'est donc : sur quel(les) URL(s) tu te connectes pour télécharger des paquets
- il existe un dossier qui contient la liste des URLs consultées quand vous demandez un téléchargement de paquets



## Très bon TP, j'ai beaucoup appris ! 
Juste quelques interrogatons sur le hash et le mdp non reconnu de marmotte.