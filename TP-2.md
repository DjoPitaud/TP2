# Partie : Files and users

- [Partie : Files and users](#partie--files-and-users)
- [I. Fichiers](#i-fichiers)
  - [1. Find me](#1-find-me)
- [II. Users](#ii-users)
  - [1. Nouveau user](#1-nouveau-user)
  - [2. Infos enregistrées par le système](#2-infos-enregistrées-par-le-système)
  - [3. Hint sur la ligne de commande](#3-hint-sur-la-ligne-de-commande)
  - [3. Connexion sur le nouvel utilisateur](#3-connexion-sur-le-nouvel-utilisateur)

# I. Fichiers

## 1. Find me

- Dans un premier temps je me connecte à ma VM en changeant son IP pour la mettre sur le même réseau puis avec la commande `$ ssh djo@192.168.56.2` je me connecte en ssh.

🌞 **Trouver le chemin vers le répertoire personnel de votre utilisateur**

![utilisateur](/TP2/img/utilisateur.png)


🌞 **Trouver le chemin du fichier de logs SSH**

grâce à la commande `cd /var/log` et `sudo nano secure`

![log ssh](/TP2/img/log%20ssh.png)

🌞 **Trouver le chemin du fichier de configuration du serveur SSH**

- Le chemin est `cd etc/ssh/sshd_config.d`
  on peut le voir avec un `ls -a` dans ssh
  ![config ssh](/TP2/img/dossier_config_ssh.png)

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

➜ **Pour le compte-rendu**, et pour vous habituer à **utiliser le terminal de façon pratique**, petit hint :

```bash
# supposons un fichier "nul.txt", on peut afficher son contenu avec la commande :
$ cat /chemin/vers/nul.txt
salut
à
toi

# il est possible en une seule ligne de commande d'afficher uniquement une ligne qui contient un mot donné :
$ cat /chemin/vers/nul.txt | grep salut
salut

# à l'aide de `| grep xxx`, on a filtré la sortie de la commande cat
# ça n'affiche donc que la ligne qui contient le mot demandé : "salut"
```

🌞 **Prouver que cet utilisateur a été créé**

- en affichant le contenu d'un fichier
- sous Linux, il existe un fichier qui contient la liste des utilisateurs ainsi que des infos sur eux (comme le chemin vers le répertoire personnel)
- utilisez une syntaxe `cat fichier | grep marmotte` pour n'afficher que la ligne qui concerne notre utilisateur `marmotte`

🌞 **Déterminer le *hash* du password de l'utilisateur `marmotte`**

- là encore, sous Linux, il existe un fichier qui liste les hashes des mots de passe de tous les utilisateurs
- encore une syntaxe `cat fichier | grep xxx` pour le compte-rendu

> **On ne stocke JAMAIS le mot de passe des utilisateurs** (sous Linux, ou ailleurs) mais **on stocke les *hash* des mots de passe des users.** Un *hash* c'est un dérivé d'un mot de passe utilisateur : il permet de vérifier à l'avenir que le user tape le bon password, mais sans l'avoir stocké ! On verra ça une autre fois en détails.

![File ?](./img/file.jpg)

## 3. Hint sur la ligne de commande

> *Ce qui est dit dans cette partie est valable pour tous les OS.*

**Quand on donne le chemin d'un fichier à une commande, on peut utiliser soit un *chemin relatif*, soit un *chemin absolu* :**

➜ **chemin absolu**

- c'est le chemin complet vers le fichier
  - il commence forcément par `/` sous Linux ou MacOS
  - il commence forcément par `C:/` (ou une autre lettre) sous Windows
- peu importe où on l'utilise, ça marche tout le temps
- par exemple :
  - `/etc/ssh/sshd_config` est un chemin absolu
  - *et c'est le chemin vers le fichier de conf du serveur SSH sous Linux en l'occurrence*
- mais parfois c'est super long et chiant à taper/utiliser donc on peut utiliser...

➜ ... un **chemin relatif**

- on écrit pas le chemin en entier, mais uniquement le chemin depuis le dossier où se trouve
- par exemple :
  - si on se trouve dans le dossier `/etc/ssh/`
  - on peut utiliser `./sshd_config` : c'est le chemin relatif de `sshd_config` quand on se trouve dans `/etc/ssh/`
  - un chemin relatif commence toujours par un `.`
  - `.` c'est "le dossier actuel"

➜ **Exemples :**

```bash
# on se déplace dans un répertoire spécifique, ici le répertoire personnel du user it4
$ cd /home/it4

# on affiche (parce que pourquoi pas) le fichier de conf du serveur SSH
# en utilisant le chemin absolu du fichier
$ cat /etc/ssh/sshd_config
[...] # ça fonctionne

# cette fois chemin relatif ???
$ cat ./sshd_config
cat: sshd_config: No such file or directory
# on a une erreur car le fichier "sshd_config" n'existe pas dans "/home/it4"

# on se déplace dans le bon dossier
$ cd /etc/ssh

# et là
$ cat ./sshd_config
[...] # ça fonctionne

# en vrai pour permettre d'aller plus vite, ça marche aussi si on met pas le ./ au début
$ cat sshd_config
[...] # ça fonctionne
```

## 3. Connexion sur le nouvel utilisateur

🌞 **Tapez une commande pour vous déconnecter : fermer votre session utilisateur**

- pas de shutdown ou reboot hein, juste fermer la session
- attention, cette commande peut varier suivant l'OS utilisé, ou la façon dont vous vous connectez à la machine (SSH ou non)

🌞 **Assurez-vous que vous pouvez vous connecter en tant que l'utilisateur `marmotte`**

- une fois connecté sur l'utilisateur `marmotte`, essayez de faire un `ls` dans le répertoire personnel de votre premier utilisateur
- assurez-vous que vous mangez un beau `Permission denied` : vous avez pas le droit de regarder dans les répertoires qui vous concernent pas

> **On verra en détails la gestion des droits très vite.**

IP VM: 10.0.2.15/24