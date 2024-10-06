# Partie : Files and users

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

🌞 **Prouver que cet utilisateur a été créé**

![config ssh](/TP2/img/user_marmotte.png)


🌞 **Déterminer le *hash* du password de l'utilisateur `marmotte`**

- Je suis allé dans `/etc/shadow` mais le mdp était en clair.
![shadow](/TP2/img/shadow_marmotte.png)
- J'ai donc vérifié si le `ENCRYPT_METHOD` était défini mais ça avait l'air good.
![SHA512](/TP2/img/SHA512.png)
Donc je sais pas pourquoi et j'ai pas creusé plus mais je veux bien une explication.


## 3. Connexion sur le nouvel utilisateur

🌞 **Tapez une commande pour vous déconnecter : fermer votre session utilisateur**

- J'ai utilisé `logout` ça m'a déci du SSH.
- Je me suis reco sur `SSH marmotte@<IP>` mais le mdp ne fonctionnait pas.
- Du coup je suis passé par `sudo su - marmotte` je sais pas si c'est de la triche ? 

🌞 **Assurez-vous que vous pouvez vous connecter en tant que l'utilisateur `marmotte`**

![triche?](/TP2/img/marmotte.png)


## Très bon TP, j'ai beaucoup appris ! 
Juste quelques interrogatons sur le hash et le mdp non reconnu de marmotte.