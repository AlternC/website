Bienvenue dans la FAQ d'AlternC. 

#### Je n'arrive pas a accéder a mon bureau.

Alternc utilise le protocole https par défaut pour l'accès au panel, il faut donc utiliser ​`https://alternc.monserveur.org` pour y accéder.

#### Sur quelle distribution je peux installer AlternC ?

AlternC est optimisé pour [Debian](https://www.debian.org/). Vous pouvez pour le moment l'installer sur la version 6  (squeeze) et 7 (wheezy).

#### Peut-on installer Alternc sous windows?

Non.

#### Je me suis trompé pour les questions posées lors de l'installation, je peux faire quoi ?

La méthode la plus simple est d'utiliser la commande `dpkg-reconfigure alternc`. 

Il est aussi possible de modifier le fichier de configuration `/etc/alternc/alternc.conf` puis de (re)lancer `alternc.install`.

#### Comment mettre à jour vers la dernière version ?

Comme pour l'installation, il faut rajouter le dépôt d'AlternC dans `/etc/apt/sources.list.d/alternc.list` : 

```
deb http://debian.alternc.org/ wheezy main
```
Et ajouter la clé d'authenfication du dépôt si ce n'est pas déjà fait : 

```
wget http://debian.alternc.org/key.txt -O - | apt-key add -
```

Il faut ensuite mettre les dépôts à jour (`apt-get update`) et lancer la mise à jour avec `apt-get upgrade`.

#### Impossible de créer un domaine/compte FTP/liste d'envoi/etc…

C'est probablement les quotas du compte qui sont a zéro. Les quotas peuvent être gérés par l'administrateur, dans le menu "Gérer les membres". Nous vous conseillons de régler des quotas par défaut dans le "panneau administrateur".

####Je viens d'installer un plugin mais aucun menu n'apparaît dans mon compte !

C'est probablement un problème de quota qui est à zéro. (Voir la réponse précédente)

#### Quelle est la correspondance entre les ID des comptes AlternC et les usagers UNIX?

Chaque compte alternc est un "group" unix. Ce groupe n'est pas (pour l'instant ?) dans `/etc/group`, les fichiers apparaissent alors comme ayant des ID "numériques" (comme 2001:2001).

#### Comment migrer un domaine existant sur un autre compte

#### Les quotas ne fonctionnent pas, que faire?

#### J'ai perdu mon mot de passe administrateur !