# FAQ d'AlternC.

N'hésitez pas à nous contacter via IRC sur le canal `#alternc` (`irc.freenode.org`).

## Dans quelles langues AlternC est il disponible ?

AlternC est pour le moment disponible en anglais et en français, et partiellement en espagnol, allemand et italien. Vous pouvez d'ailleurs nous aider à finir la traduction dans d'autres langues sur [transifex](https://www.transifex.com/octopuce/alternc/).

## Sur quelle distribution je peux installer AlternC ?

AlternC est optimisé pour [Debian](https://www.debian.org/). Vous pouvez pour le moment l'installer sur la version 6 (Squeeze) 7 (Wheezy), 8 (Jessie) et 9 (Stretch). La version pour Debian 9 (Stretch) existe mais n'a pas été testée extensivement à ce jour.

## Peut-on installer AlternC sous windows?

Non.

## Peut-on installer AlternC sous Centos, Fedora, Ubuntu server, Archlinux, etc. (ou tout autre distribution Linux) ?

A priori, le panel d'AlternC n'interdit pas de fonctionner sur une autre distribution. Toutefois, nous ne fournissons pas de package pour ces autres distributions. Cela requiert de regarder le code source et le Makefile pour savoir où déployer les configurations, quelles dépendances installer etc. 

Sur une distribution basée sur Debian, (y compris Debian/KFreeBSD, Devuan ou Archlinux), cela devrait marcher à partir des dépôts AlternC sans problème, mais cela n'a pas été testé.

## Je me suis trompé lors des questions pendant l'installation, je peux faire quoi ?

La méthode la plus simple est d'utiliser la commande `dpkg-reconfigure -plow alternc` puis de relancer `alternc.install`. 

Il est aussi possible de modifier le fichier de configuration `/etc/alternc/alternc.conf` puis de (re)lancer `alternc.install`.

## Comment mettre à jour vers la dernière version ?

Comme pour l'installation, il faut rajouter le dépôt d'AlternC dans `/etc/apt/sources.list.d/alternc.list` : 

```
deb http://debian.alternc.org/ jessie main
```

Et ajouter la clé d'authenfication du dépôt si ce n'est pas déjà fait : 

```
wget https://debian.alternc.org/key.txt -O - | apt-key add -
```

Il faut ensuite mettre les dépôts à jour (`apt-get update`) et lancer la mise à jour avec `apt-get upgrade`.

## Impossible de créer un domaine/compte FTP/liste d'envoi/etc…

C'est probablement les quotas du compte qui sont a zéro. Les quotas peuvent être gérés par l'administrateur, dans le menu "Gérer les membres". Nous vous conseillons de régler des quotas par défaut dans le "panneau administrateur".

## Je viens d'installer un plugin mais aucun menu n'apparaît dans mon compte !

C'est probablement un problème de quota pour ce service qui est à zéro. (Voir la réponse précédente)

## Quelle est la correspondance entre les ID des comptes AlternC et les usagers UNIX?

Chaque compte AlternC est un utilisateur et un groupe Unix. Ce groupe n'est pas défini pour l'instant dans `/etc/group` ou `/etc/passwd`, les fichiers apparaissent alors comme ayant des ID numériques (comme 2001:2001).

## J'ai perdu mon mot de passe administrateur !

Vous pouvez réinitialiser le mot de passe de l'administrateur (admin) via la ligne de commande `/usr/lib/alternc/alternc-passwd`.

