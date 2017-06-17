
Pré-installation
================

AlternC est prévu pour fonctionner sur la distribution Linux Debian Squeeze, Wheezy (ou Jessie en pré-release).

Pour installer Alternc vous devez : 

* avoir un accès SSH à votre serveur.
* Avoir les droits d'administrateur (`sudo -s` ou `su`)
* vérifier que `#includedir /etc/sudoers.d` est présent dans `/etc/sudoers` avec la commande `visudo`.

Installation facile
===================

Pour installer AlternC en quelques questions, [utilisez l'easy-install, un shell-script permettant de tout installer rapidement](https://github.com/AlternC/easy-install)

Vous pouvez aussi installer AlternC manuellement, grâce à la documentation ci-dessous : 

Installation manuelle
=====================

ACL
---

AlternC dépend des acls noyau afin de gérer les droits utilisateurs notamment en ce qui concerne les dossiers web. Il est donc nécessaire d'installer le paquet `acl` avec :

```
apt-get install acl
```

Il faut ensuite indiquer au système la partition qui va contenir les données utilisateurs en y activant les ACLs. Pour ce faire, on peut modifier le fichier `/etc/fstab` en rajoutant l'option "acl" à la partition concerné, par exemple :

```
/dev/md1    /    ext3    auto,noatime,acl    0    0
```

> ATTENTION : c'est `acl` si votre système de fichier est en `ext4`, mais c'est `attr2` pour du `xfs`.


Quota
-----

AlternC peut également gérer les quotas disques des utilisateurs. Contrairement aux ACLs, les quotas ne sont pas nécessaires au fonctionnement d'AlternC. S'ils ne sont pas activé ou installé, AlternC considérera simplement que les quotas d'espace web sont infinis pour chaque utilisateur. Pour ce faire il faut installer le paquet quota :

```
apt-get install quota
```

Et encore une fois modifier le `/etc/fstab` pour indiquer leur activation :

```
/dev/md1 /               ext3    acl,grpquota,errors=remount-ro 0       1
```

Remontage de la partition
-------------------------

Une fois ces modifications effectués, il suffit de remonter la partition concerné -en supposant toujours que c'est la partion `/` qui contiendra les données utilisateurs avec :

```
mount -o remount,acl,grpquota /
```



Installation
============

MySQL
-----

Il est nécessaire d'installer MySQL pour pouvoir utiliser AlternC. Si vous voulez l'héberger sur la même machine, vous pouvez le faire avec la commande suivante :

```
apt-get install mysql-server
```

> **IMPORTANT** : Entrez un mot de passe administrateur et NOTEZ-LE car il vous sera demandé en cours d'installation.

Idéalement, stockez ce mot de passe dans un fichier /root/.my.cnf, contenant les lignes suivantes, ce qui vous permettra de vous connecter en tant qu'administrateur sur votre MySQL via une simple commande `mysql`: 

```
[client]
user=root
password=<votre mot de passe>
```


Configuration des dépôts
------------------------

Pour installer AlternC sur un serveur vous devez utiliser un éditeur de texte pour ajouter la source des paquets d'AlternC :

```
deb http://debian.alternc.org/ wheezy main
```

dans le fichier `/etc/apt/sources.list.d/alternc.list`.

Les paquets Debian sont signés numériquement, avant d'exécuter un apt-get update, il convient d'ajouter la clef du dépôt d'AlternC avec la commande :

```
wget http://debian.alternc.org/key.txt -O - | apt-key add -
```

Il s'agit d'une clé PGP possédée et maintenue par les développeurs ayant le droit d'écrire dans le dépot sur [debian.alternc.org](http://debian.alternc.org).


Installation
------------

Ensuite, mettez à jour la liste des packages disponibles pour apt :

```
apt-get update
```

Il ne reste qu'à lancer la commande pour installer AlternC (n'oubliez pas la partie *finaliser l'installation* après cette opération) :

```
apt-get install alternc alternc-ssl alternc-api
```

Vous pouvez y ajouter 

* `alternc-mailman` pour gérer des listes de discussion ou de diffusion avec [Mailman](http://www.gnu.org/software/mailman/),
* `alternc-roundcube` pour pouvoir utiliser un webmail avec [Roundcube](https://roundcube.net/),
* `alternc-awstats` pour générer des statistiques pour vos sites web avec [Awstats](http://www.awstats.org/),

Ou d'autres greffons à AlternC 

DNS - serveurs de nom
---------------------

Les serveurs de noms servent à distribuer l'information sur les noms de domaine installés sur votre serveur. Si vous avez besoin de serveurs de noms, AlternC vous propose un service gratuit sur [alternc.net](https://alternc.net). Dans ce cas vous pouvez saisir :

* DNS primaire : `ns1.alternc.net`
* DNS secondaire : `ns2.alternc.net`

Nom de domaine du serveur
-------------------------

Attention, si vous avez un nom de domaine que vous comptez utiliser pour héberger un site, ne l'indiquez pas dans cet écran. En effet, ce nom de domaine sera alors la "porte d'accès" à AlternC.

Utilisez un autre nom de domaine pointant sur votre machine ou un sous-domaine comme `panel.votredomaine.com`.

En résumé, si votre serveur est sur l'IP 12.34.56.78 et que vous avez le nom de domaine exemple.com pour votre site perso, n'utilisez pas exemple.com mais soit l'adresse IP, soit panel.exemple.com, soit le domaine fourni par votre hébergeur ex: serveur234215.groshebergeur.net

* phpMyAdmin

Lors de l'installation, Debian vous demande de configurer PhpMyAdmin, vous pouvez le laisser faire, il vous demandera le mot de passe de l'administrateur de la base de données MySQL pour s'installer. Cependant, AlternC se chargera de modifier quelques peu ces paramètres par la suite.

* Postfix

Choisir "Site Internet", puis suivre les instructions, notamment en rappelant le nom de domaine complet de votre serveur (cf plus haut)

Finaliser l'installation
------------------------

Une fois que l'installation est achevée, le script `alternc.install` **doit être exécuté**. Il va générer notamment les configurations de votre serveur pour qu'AlternC fonctionne.

```
alternc.install
```

Si vous souhaitez installer un certificat SSL pour votre panel (qui sera aussi utilisé par Dovecot, Proftpd, Postfix etc.) nous vous recommandons d'installer LetsEncrypt et de configurer un certificat [grâce à notre documentation en ligne sur SSL](SSL-fr).


Post Installation
=================

Première connexion
------------------

Vous pouvez désormais accéder au panel AlternC sur le nom de domaine ou l'IP que vous avez donné. Vous devriez voir une page de login dont l'**accès par défaut que vous  devez changer immédiatement** est :

* identifiant : `admin`
* mot de passe : `admin`

N'oubliez pas de modifier votre mot de passe administrateur en vous rendant dans le menu `Gérer les comptes AlternC` !

