
AlternC comme si vous y étiez, le livre :
* [Introduction par Laurent Chemla, Chantal Bernard-Putz & Benjamin Sonntag](Book-intro-laurent-chemla-fr)
* [Chapitre 3-4, Un peu d'histoire](Book-chapitre-3-histoire)
* [Chapitre 5, guide utilisateur](Book-chapitre-5-guide-utilisateur-fr)
* [Chapitre 6, guide administrateur](Book-chapitre-6-guide-administrateur-fr)
* [Chapitre 7-8, guide avancé, trucs & astuces](Book-chapitre-7-guide-avance-fr)
* [Annexe, ils utilisent et contribuent à AlternC](Book-annexe-ils-utilisent-alternc-fr)


Chapitre VII - Technique : Administration Système et AlternC
============================================================

Installation d'AlternC
----------------------

L'installation d'AlternC est relativement aisée, elle utilise le système
de paquets logiciels de Debian, qui signe numériquement les logiciels
fournis, et résout pour vous les dépendances entre programmes.

Ainsi, lorsque vous installerez AlternC, tous les logiciels nécessaires
au bon fonctionnement de votre serveur web et email seront
automatiquement installés.



### Prérequis

Pour installer et configurer correctement votre serveur, vous devez :

* avoir un serveur en Debian Stretch (la version stable à ce jour)
* savoir éditer votre fichier sources.list
* connaître les commandes de base concernant apt-get; apt ou aptitude
* être possesseur d'un nom de domaine, et avoir un sous-domaine qui
    pointe vers l'IP de votre serveur (les exemples utiliseront
    demo.alternc.org )

Les commandes de ce chapitre devront être saisies telles quelles dans
une console connectée à votre serveur sur le compte root (d'où le \# en
début de ligne qui signale une console administrateur).

Le serveur AlternC que nous allons installer s'appellera
demo.alternc.org.

Tout d'abord nous allons configurer debconf

debconf est l'outil de Debian qui vous interroge lors des installations
de package.

Nous avons choisi de lui demander de poser toutes les questions, et pas
seulement les questions critiques.

--------------------------------------------------------------------------

#### code

\# sudo dpkg-reconfigure debconf

--------------------------------------------------------------------------

![img](images/10000201000002B9000001DE61180261.png)


Ces panneaux sont gris clair et sur fond bleu, ils s'appellent les
questions debconf, et exigent de votre part la plus grande attention :
ce sont les réponses à ces questions qui détermineront les paramètres
par défaut des services que vous installerez. Ici, debconf demande
quelle interface utiliser pour répondre aux questions. Nous choisissons
« dialogue » (ou dialog en anglais)

![img](images/10000201000002B9000001DEFE54E86D.png)

Ensuite, debconf demande quelle priorité doivent avoir les questions
auxquelles il faudra répondre pour installer un package.

Choisir « basse » permet d'afficher l'ensemble des questions posées par
le système.

Ensuite, installer apache2, postfix et default-mysql-server-core, ce
qui signale à AlternC que l'installation concernera un unique serveur.
En effet, il est possible d'installer AlternC en le répartissant sur
plusieurs machines et mettre postfix ou mariadb sur une autre machine.
Apache2 doit être installé pour signaler à AlternC l'utilisation
de cette version d'apache (AlternC est compatible avec apache 2.4)

--------------------------------------------------------------------------

#### code pour installer apache2 et Mariadb

\# sudo apt install -y apache2 libapache2-mpm-itk postfix default-mysql-server-core

--------------------------------------------------------------------------

![img](images/1000000000000267000000F4FE63C62F.jpg)

Debconf demandera plusieurs fois, pendant cette installation, de définir
un mot de passe pour Mariadb : ne jamais remplir ce formulaire, sans quoi
AlternC ne pourra pas à s'installer. Le mot de passe général Mariadb sera
défini à l'issue de l'installation.

Lors de l'installation de Postfix, debconf posera de nombreuses
questions.

La question portant sur le type de configuration du serveur de mail
postfix est la plus importante : répondre « Site Internet » ici, sans
quoi AlternC ne pourra pas s'installer faute d'une configuration de
postfix correcte.

![img](images/10000201000002B9000001DE90BC7CA0.png)

Puis, debconf demande le nom de domaine du serveur. Entrer le nom du
sous-domaine qui pointe vers votre serveur, et qui sera l'adresse de
votre panel AlternC :

![img](images/10000201000002B9000001DEBE4D3AEC.png)

Ensuite, Debconf posera probablement de nombreuses questions. À
l'exception de celles qui seront présentées ici, valider toujours la
valeur par défaut proposée par Debian : ces valeurs sont bien pensées
pour obtenir un serveur stable et performant.

Postfix finit donc de s'installer et apt rend la main ...

### 

### Installation d'AlternC

Une fois ces pré-requis satisfaits, il est possible d'installer
AlternC :

--------------------------------------------------------------------------

#### code pour installer AlternC

\# sudo apt install -y alternc

--------------------------------------------------------------------------

Debian propose alors d'installer toutes les dépendances et pose les
questions des packages en dépendance, ainsi que les questions
d'installation d'AlternC lui-même.


![img](images/10000000000002E2000001CDF0936805.png)

*Rappel* : Debconf posera probablement de nombreuses questions pendant
l'installation. À l'exception de celles qui seront présentées ici,
valider toujours la valeur par défaut proposée par Debian : ces valeurs
sont bien pensées pour obtenir un serveur stable et performant.

Lors de l'installation de PhPMyAdmin, debconf demande quel serveur web
configurer. Cocher « apache2 » puis OK.

![img](images/10000000000002A0000000F3C360DC1E.jpg)

Au bout d'un certain temps, les questions posées par debconf
concerneront AlternC lui-même (le titre en haut du panneau de question
le prouve)

![img](images/100000000000029D000000FA81F0C7C5.jpg)

L'installation d'AlternC commence donc. Renseigner les informations avec
soin :

Tout d'abord le nom DNS du panel. C'est cette adresse, en http ou https,
qui donnera accès au panel AlternC d'administration de votre serveur.

![img](images/10000000000002A2000000E3EDCCE3C6.jpg)

Ensuite, debconf demande le nom du service d'hébergement. C'est ce nom
qui sera affiché dans le menu principal d'AlternC et sur la fenêtre de
login.

![img](images/10000000000002A2000000B50768F130.jpg)

![img](images/10000000000002A1000000DF61BCBCBF.jpg)

Renseigner ici l'IP du serveur. S'il est derrière un NAT, renseigner ici
l'IP natée (192.168.X.Y)

![img](images/100000000000029C000000B02978139D.jpg)

Renseigner ici l'IP publique du serveur, donc typiquement pas une IP en
10.x ou 192.168.X ou 172.X, mais bien une IP joignable sur Internet.

![img](images/100000000000028D000000B5BA3DC126.jpg)

Ce paramètre est le nom du serveur MX, à savoir la machine qui recevra
les mails. Laisser la valeur par défaut qui doit être celle du nom du
sous-domaine contenant le panel.

Les deux paramètres qui suivent correspondent en général aux DNS
primaire et secondaire de l'hébergeur du serveur.

Le primaire peut parfois être votre serveur lui-même.

![img](images/100000000000029F000000DB35CED622.jpg)

Le secondaire devrait, idéalement, faire tourner le package
alternc-slavedns (voir le chapitre correspondant dans ce livre) pour
synchroniser la liste des domaines hébergés avec les serveurs AlternC
dont il est le slave dns.

![img](images/1000000000000287000000EF22ECBE48.jpg)

Si vous avez besoin d'un service de DNS secondaire pour AlternC, vous
pouvez contacter la liste de discussion dev@alternc.org : nous disposons
d'un tel service sur ns2.alternc.org, service que nous mettons
gracieusement à disposition à nos utilisateurs.

![img](images/100000000000029D000000D0A722A2F7.jpg)

Ces paramètres correspondent à la configuration de Mariadb et ne doivent,
en général, pas être modifiés.

![img](images/100000000000029B000000FE95EA9E5C.jpg)

![img](images/100000000000029F000000B2C4285246.jpg)

![img](images/100000000000027C000000B45F68022D.jpg)

Ici, enfin,il est possible d'indiquer un mot de passe Mariadb... ou de
laisser AlternC en génèrer un aléatoire.

![img](images/1000000000000294000000CC70608EB8.jpg)

![img](images/100000000000029A000000C7951FCEA5.jpg)

![img](images/10000000000002A0000000FC2AD1410D.jpg)

Si vous utilisez un serveur de supervision (comme Nagios par exemple),
renseigner son IP ici : cela autorisera ce serveur de supervision à
utiliser les ressources de votre serveur.

![img](images/1000000000000298000000BBF6B579A1.jpg)

La question suivante est particulière : elle ne sert que si vous
utilisez une ferme de serveurs « plusieurs serveurs sous AlternC qui se
répartissent la charge. Cette fonction est en pré-version, et nécessite
de très bien connaître AlternC pour être utilisée. Laissez donc ce champ
vide à ce jour.

![img](images/100000000000029C000000D21800C707.jpg)

Une fois l'installation terminée, vous devez voir un message de
l'installeur d'AlternC qui vous signale que le compte administrateur
d'AlternC, nommé « admin », a été créé avec succès.

Ce compte admin a pour mot de passe « admin », connectez-vous donc au
panel et modifiez-le dès que possible !

Enfin, comme nous n'avons défini aucun mot de passe pour le serveur
Mariadb de votre serveur, il faut sécuriser notre serveur de base de
données. Pour cela lancer la commande suivante :

--------------------------------------------------------------------

### code MySQL (en tant que root)

\# mysql_secure_installation

--------------------------------------------------------------------

Note: running all parts of this script is recommended for all mysql
servers in production use! please read each step carefully!

In order to log into Mariadb to secure it, we'll need the current password
for the root user. If you've just installed Mariadb, and you haven't set
the root password yet, the password will be blank, so you should just
press enter here.

Enter current password for root (enter for none): **(taper enter ici)**

OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the Mariadb
root user without the proper authorisation.

Set root password? \[Y/n\] **(taper y et enter)**

New password: **(entrez un mot de passe neuf)**

Re-enter new password: **(confirmez votre mot de passe)**

Password updated successfully!

Reloading privilege tables.. ... Success!

By default, a Mariadb installation has an anonymous user, allowing anyone
to log into Mariadb without having to have a user account created for
them. This is intended only for testing, and to make the installation go
a bit smoother. You should remove them before moving into a production
environment.

Remove anonymous users? \[Y/n\] **(taper y)**

... Success!

Normally, root should only be allowed to connect from 'localhost'. This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? \[Y/n\] **(taper y)**

... Success!

By default, Mariadb comes with a database named 'test' that anyone can
access. This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? \[Y/n\] **(taper y)**

* Dropping test database...

* Removing privileges on test database...

... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? \[Y/n\] **(taper y)**

... Success!

Cleaning up...

All done! If you've completed all of the above steps, your Mariadb
installation should now be secure. Thanks for using Mariadb!

Ainsi, votre serveur Mariadb n'aura pas de compte sans mot de passe, et ne
sera donc pas ouvert à tous sans authentification.

### Liste des ports en écoute sur le serveur

Une fois votre serveur installé, vous pouvez taper la commande suivante,
qui vous montre la liste des processus et des ports de communication en
attente sur votre serveur. Cela vous permet de constater l'ouverture des
ports suivants :

TCP : 25 110 143 993 995 : pour les services SMTP IMAP POP

TCP : 80 443 : pour les services HTTP et HTTPS

TCP: 21 : pour le FTP

TCP : 3306 : pour MySQL / Mariadb (noter qu'il n'écoute que sur 127.0.0.1, ce
qui rend votre serveur de base de données non joignable de l'extérieur,
ce qui est bien en terme de sécurité)

UDP et TCP : 53 : Pour le service de nommage des domaines (le DNS)

TCP 22 : Le SSH

![img](images/10000000000002A00000012860AC126F.png)

### Amorcer son premier serveur

Pour faire pointer le sous-domaine de votre panel sur votre serveur,
utilisez les serveurs DNS de votre registrar, et faites pointer le
sous-domaine vers l'adresse IP de votre serveur.

Installation des modules d'AlternC
----------------------------------

Une fois AlternC installé avec succès (et seulement une fois cela fait)
vous pouvez ajouter des extensions, appelés modules, qui sont
installables sous forme de package debian à part.

### Installation du paquet des statistiques web Awstats

Le package AlternC-awstats permet d'utiliser le logiciel de statistiques
web Awstats[2] pour générer des pages de statistiques pour vos sites
web. Pour l'installer, taper :

----------------------------------
##### Code Awstats

\# sudo apt install -y alternc-awstats

----------------------------------

Il vous demande confirmation de l'installation, et installe en même
temps les dépendances.

![img](images/10000000000002F8000000E945AA330A.png)

### Installation du paquet Mailman pour AlternC

AlternC-mailman est un package permettant d'utiliser dans vos noms de
domaines hébergés des listes de discussion et de diffusions par mail
utilisant le logiciel Mailman[3]. Pour cela, taper :

----------------------------------
##### Code Mailman

\# sudo apt install -y alternc-mailman

----------------------------------

![img](images/10000000000002F7000000DE1358085D.png)

Après vous avoir confirmé l'installation, notamment des dépendances,
debconf vous pose quelques questions relatives au package *Mailman* :

![img](images/100000000000029E00000179F80C0BC3.jpg)
Cochez les langues que vous souhaitez utiliser dans votre gestionnaire
de listes de discussions.

![img](images/10000000000002A300000106C2028617.jpg)

et choisissez la langue par défaut des nouvelles listes.

![img](images/10000000000002A2000000DD0733E0C9.jpg)

![img](images/10000000000002A2000000E33F19EF9E.jpg)

Ne vous inquiétez pas de l'absence de la liste « mailman » principale :
l'installation du package alternc-mailman créera cette liste principale
si besoin.

![img](images/10000000000002A10000016EEBDAB802.jpg)

Ce paramètre est important : il est fourni par alternc-mailman pour vous
proposer de modifier Mailman pour supporter les « listes virtuelles ».
Dans ce mode (si vous cochez oui) vous pourrez avoir, sur ce serveur,
des listes ayant la même partie gauche de l'arobase et appartenant à des
domaines différents. Par exemple :

discussion@mondomaine.fr

et

discussion@autredomaine.net

pourront être hébergées sur le même serveur.

Sans cela, Mailman interdit plusieurs listes du même nom sur le même
serveur.

Sur un serveur neuf, il n'y a aucune raison de ne pas profiter de cette
possibilité.

Sur un serveur existant, vous feriez mieux de conserver le paramètre
choisi jusque-là, sinon votre mailman risque d'avoir un comportement
étrange ...

Installation de Squirrelmail pour AlternC
=========================================

-------------------------------------------------------------------------

#### code Squirrelmail

sudo apt install -y alternc-squirrelmail

-------------------------------------------------------------------------

### Installation du module de changement de mot de passe dans Squirrelmail pour AlternC

Ce module permet de laisser aux utilisateurs la possibilité de changer
leur mot de passe depuis le webmail squirrelmail.

Le webmail permet à tout utilisateur disposant d'un compte POP/IMAP sur
le serveur de lire son courrier à travers une interface web simplissime.
Grâce à ce module, vous pourrez modifier votre mot de passe en cliquant
sur « options » puis « changer mon mot de passe » dans le webmail

----------------------------------

##### Code Changepass ( paquet à revoir )

\# sudo apt install -y alternc-changepass

----------------------------------

![img](images/100000000000029E000000B9434EDA0B.jpg)

Une fois le module installé, il faut l'activer dans Squirrelmail. 
Pour cela, taper la commande suivante :

----------------------------------

##### instruction (n°1) perl pour Squirrelmail

\# sudo  /etc/squirrelmail/conf.pl

----------------------------------

![img](images/100000000000027200000169CBFD50F3.png)

Puis aller dans le menu 8 (plugins) et taper le numéro du plugin
alternc_changepass.

taper ensuite S pour sauver et Q pour quitter la configuration de
Squirrelmail

![img](images/10000000000002720000024182AEB2EE.png)

![img](images/10000000000002710000024265D7A801.png)

Et voilà ! vos utilisateurs peuvent désormais modifier eux-même leur mot
de passe de messagerie. Notez que le module de politiques de mot de
passe est également installé.

### Installation de Procmail

Le module AlternC-procmail utilise le logiciel Procmail pour
permettre aux utilisateurs du service de courrier électronique
(notamment ceux utilisant IMAP ou le webmail intensivement) de définir 
des filtres sur leur boite aux lettres. Pour l'installer, c'est aussi simple que pour le
précédent :

----------------------------------

#### code Procmail

\# sudo apt install -y alternc-procmail

----------------------------------

![img](images/10000000000002EF000000E5FB98B640.png)

Une fois le module installé, il faut l'activer dans Squirrelmail. Pour
cela, taper la commande suivante :

----------------------------------

##### instruction (n°2) perl pour Squirrelmail

\# sudo /etc/squirrelmail/conf.pl

----------------------------------

![img](images/100000000000027400000175E582C339.png)

	puis allez dans le menu 8 (plugins) et taper le numéro du plugin
alternc\_changepass.

taper ensuite S pour sauver et Q pour quitter la configuration de
Squirrelmail

![img](images/10000000000002730000023319F2F03B.png)

![img](images/100000000000027300000232B1308D80.png)

Et voilà ! Vos utilisateurs peuvent désormais aller dans le menu
« options » de leur compte dans le webmail Squirrelmail, puis choisir
« filtrer les messages ». Une interface web simple leur propose alors de
créer des filtres de message parmi lesquels :

\- le fait de classer un mail dans un des dossiers IMAP en fonction du
destinataire, du sujet ou de l'expéditeur

\- le fait de répondre automatiquement avec un message prédéfini
(typiquement un message d'absence du bureau, ou de vacances, ou de
remplacement d'un service par un autre, un message d'accusé de réception
etc.). C'est la fonction bien connue de répondeur automatique.

Certificats SSL
---------------

### Introduction

Après l'installation d'un AlternC de base, il n'est pas possible de se
connecter au bureau en HTTPS, mais uniquement en HTTP non sécurisé. En
effet, pour activer le HTTPS, il faut disposer d'un certificat client,
idéalement un certificat reconnu par une autorité de certification. Nous
allons donc vous expliquer comment générer un certificat SSL pour
AlternC, qui sera utilisé par tous les services sachant utiliser la
cryptographie, à savoir :

\- le HTTPS pour le panel

\- le POPS et l'IMAPS pour le courrier des utilisateurs

\- le SMTPS pour le courrier sortant et entrant

\- le FTPS pour les transferts de fichier

\- d'autres services utilisant la cryptographie, comme Jabber.

### Les fichiers de certification

Le principe du certificat de sécurité SSL est un peu compliqué, voici
donc une petite introduction qui vous permettra de mieux comprendre de
quoi nous parlerons ensuite.

Le principe de cryptographie utilisé sur ce type de service est le
suivant : vous générez une paire de clé RSA privée et publique, qui va
vous permettre de vous identifier auprès des clients du réseau, et qui
va permettre aux clients du réseau de chiffrer des données à votre
destination, et de vérifier votre identité.

Une clé RSA peut être protégée par un mot de passe. Elle ressemble à
ceci :

-----BEGIN RSA PRIVATE KEY-----

MIICXQIBAAKBgQDQe9Ckq4c9RdjqIgHa5fEwV/nZhSxWrMUdA1z1jtFnH1xNtxDR

gaEnxPCGyIi/Q7/9u+nrAdBSqgFvl+lVkQmiIXeWJd85ila1aPxCFJx/CmoNOcW/

fGY+fSLJ8gUpHxZctzEA+Eq5rxd5RzCzwjkD/2/GUfV0stkVrxPTrV+ENwIDAQAB

AoGAV4OBw33VGR1WvoFj8fUP11E2qd5DDGcdnt4oiVNYB0ecwWqU/rzBXyZGKO3I

qTDRSLs2p9VtEyYV0DgeVDK7rbcVI71yWfks4HfV9gNr4VMnBGi3XoajhGh23OOT

LtlmudVSVbcmtHGzpxjL0hP07G/IeBDbeoIIwIj35QxFViECQQDo3lJtVPomItfe

rWeT4+sG95c6FzoQf8kIMiYs0S19i266zOuR1jfLpHTT3QSiuKDsHVHKJHtTezQx

8C/nrthpAkEA5TFnun5N8y+pm5gSfd/cJEZDTcI4F0rWy28cPOk3iSWnR9vR3Tqv

e485kDQJWm8+A3mwgLYDF5ullM8WgpLjnwJBAKgSVkGs9ILz4QRJ+ZkHBknYB2EZ

BIKGCE2PDHiKXR/gtAHFBz0R9OFZCC0lt0POp+7i1nZkUFKXeLuWVK0OSAkCQQCv

rttQPRkLXHGqbztdeYeJ1sIojMPqNtfHQPhoSGzlmBNOJdYMurtg/zykqP6yb6sd

ht4hC2P7aLfYBZW/qVEJAkADoNCKAAiO7GKE9MnnIg2j1Jh7tzMiGWYWyG3Gfy/8

rkknsOWKG/r3RuZXm1XxVP1Jq/75ukZESmktZMYMcYXz

-----END RSA PRIVATE KEY-----

\- vous générez une requête de certificat, appelée Certificate Request,
ou CSR, basée sur cette paire de clé.

\- vous envoyez cette requête de certificat à une autorité de
certification. Une requête de certificat ressemble à ceci :

-----BEGIN CERTIFICATE REQUEST-----

MIIBhDCB7gIBADBFMQswCQYDVQQGEwJBVTETMBEGA1UECBMKU29tZS1TdGF0ZTEh

MB8GA1UEChMYSW50ZXJuZXQgV2lkZ2l0cyBQdHkgTHRkMIGfMA0GCSqGSIb3DQEB

AQUAA4GNADCBiQKBgQDQe9Ckq4c9RdjqIgHa5fEwV/nZhSxWrMUdA1z1jtFnH1xN

txDRgaEnxPCGyIi/Q7/9u+nrAdBSqgFvl+lVkQmiIXeWJd85ila1aPxCFJx/CmoN

OcW/fGY+fSLJ8gUpHxZctzEA+Eq5rxd5RzCzwjkD/2/GUfV0stkVrxPTrV+ENwID

AQABoAAwDQYJKoZIhvcNAQEFBQADgYEAae5QhvaCbYECHwy6nomxjAX2QFPre/PR

dlbMzP4KPGkKokbsNRXh7AY2qIbIrT9psVvL0ynaF8L5IFpzN6Upr1SHN1/KtZyM

NdUWfXyBbzFkb4XcbrHDtPbtpOr5ZNLNr6rwXRMvFgmtuqgL/Bn+/Y+LKDuEnDo8

sz1/2ZZrl5U=

-----END CERTIFICATE REQUEST-----

\- l'autorité de certification vérifie le nom de domaine et si besoin
d'autres informations relatives à ce domaine, et vous retourne un
certificat SSL.

\- l'autorité a peut-être un certificat intermédiaire, le sien, que vous
devez télécharger de même.

\- vous installez tout cela (certificat intermédiaire, certificat et clé
privée RSA) dans votre serveur aux bons endroits, et configurez les
services pour proposer du SSL/TLS.

Un certificat, qu'il soit intermédiaire ou terminal (le votre) ressemble
à ceci :

-----BEGIN CERTIFICATE-----

MIIFezCCBGOgAwIBAgIDRO0NMA0GCSqGSIb3DQEBBQUAMIHKMQswCQYDVQQGEwJV

UzEQMA4GA1UECBMHQXJpem9uYTETMBEGA1UEBxMKU2NvdHRzZGFsZTEaMBgGA1UE

ChMRR29EYWRkeS5jb20sIEluYy4xMzAxBgNVBAsTKmh0dHA6Ly9jZXJ0aWZpY2F0

ZXMuZ29kYWRkeS5jb20vcmVwb3NpdG9yeTEwMC4GA1UEAxMnR28gRGFkZHkgU2Vj

GjAYBgNVBAMTEXd3dy5zb3MtbWFpbnMuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOC

AQ8AMIIBCgKCAQEA2qhZ8zYQQwm2Bdck/YbaRjYZagk0UTt5bAChzG0VvcqHu4pC

...

...

A1UdIwQYMBaAFP2sYTKTbEXW4u6FX5q653aZaMznMCsGA1UdEQQkMCKCEXd3dy5z

b3MtbWFpbnMuY29tgg1zb3MtbWFpbnMuY29tMA0GCSqGSIb3DQEBBQUAA4IBAQA2

HIOAlzPf9QsXRDTrCQJDeHVSJlka3p7fpDd93yMOnmcoyrcEJsAMtAX4OeWqy13U

EGokjN9WPwcU7sn5lSEgea9bUepXriAd4JuPH8fD6ZsXfpf3zWWdUma0KE8XHMIi

wtHP8PVvflu4KKpE2muhkEbExHXAs8hhVTgJ1MXZ13rxlDxUbYDKsaBQuGYeToag

EEDOZ0Ddu3VNV2Gm8WLoPc8SnDNTDbNZwV2V1SMU3B4Bf2XTfpndEjbjxAsyZB3A

s7jM9Py5QmzB+yukdViS

-----END CERTIFICATE-----

Pour gérer tout cela, on utilisera la ligne de commande ***openssl***
accompagné d'un fichier de configuration *openssl.cnf*.

### Autorité intermédiaire

Si votre autorité de certification vous fournit un certificat dit « de
classe 3 », cela signifie que vous devrez probablement installer un
certificat d'autorité intermédiaire dans votre serveur.

Ce certificat intermédiaire est fourni par votre autorité, c'est un
fichier au format X.509 PEM (son nom se termine en général soit par
.CRT, soit par .PEM)

Afin que votre certificat soit bien reconnu par les clients en face, il
faut installer ce certificat intermédiaire dans votre serveur aux bons
endroits. Pour cela, procédez ainsi :

\- copiez ce certificat intermédiaire dans /etc/ssl/certs sous un nom
explicite (par exemple, nous qui utilisons Gandi comme autorité de
certification, nous avons un fichier GandiStandardSSLCA.pem)

\- taper c\_rehash. Cette commande d'openssl recrée des liens
symboliques vers votre certificat dans /etc/ssl/certs.

\- ajoutez ce certificat à votre ensemble de certificats en tapant

----------------------------------------------------------------
#### instruction certificat Gandi

sudo cd /etc/ssl/certs

sudo cat GandiStandardSSLCA.pem &gt;&gt;ca-certificates.crt

----------------------------------------------------------------
Cela ajoute votre certificat intermédiaire à la liste de tous les
certificats reconnus sur le système. Il est nécessaire pour Postfix.

Si vous utilisez Postfix, ce qui est le cas par défaut dans AlternC, il
faut reconfigurer et redémarrer Postfix comme suit :

dans /etc/postfix/main.cf, assurez-vous d'avoir les lignes suivantes :

----------------------------------
#### instructions Postfix (main.cf)

smtpd\_tls\_CAfile = /etc/ssl/certs/ca-certificates.crt

smtp\_tls\_CAfile = /etc/ssl/certs/ca-certificates.crt

----------------------------------

et redémarrez Postfix : en effet, comme postfix est protégé par un
chroot, son script de démarrage recopie ca-certificates.crt dans le
dossier

/var/spool/postfix/etc/ssl/certs/ pour que ses programmes puissent y
accéder

----------------------------------
#### instructions pour relancer Postfix

\# sudo service postfix restart

\# sudo service postfix status

----------------------------------

### 

### Configurer AlternC et Debian à l'achat d'un certificat

Vous allez donc procéder à l'acquisition (payante via une autorité
reconnue ou gratuite via CaCert par exemple) d'un certificat de sécurité
pour votre serveur. Pour cela, on procède ainsi :

Tout d'abord modifiez le fichier */etc/ssl/openssl.cnf* de votre serveur
pour que les clés que openssl génère soient au minimum de 2048 bits.
(2048 est bien, vous pouvez mettre 4096, mais évitez d'aller au-delà).

----------------------------------
#### instruction pour un certificat Openssl

~\#sudo sensible-editeur /etc/ssl/openssl.cnf

----------------------------------

\[ req \]

default\_bits = 2048

default\_keyfile = privkey.pem

distinguished\_name = req\_distinguished\_name

attributes = req\_attributes

x509\_extensions = v3\_ca \# The extentions to add to the self signed
cert

Ensuite, allez dans votre dossier alternc et lancez une création de
paire de clé RSA et de requête de certificat comme suit (les commandes à
taper ou instructions sont en gras) :

----------------------------------
#### instruction pour un certificat Openssl (RSA)

~\#sudo cd /etc/alternc

/etc/alternc/\#sudo openssl req -new

----------------------------------

Generating a2048 bit RSA private key

..................................................................++++++
...............................…++++++

writing new private key to 'privkey.pem'

Enter PEM pass phrase: **(entrez ici un mot de passe temporaire pour
votre clé RSA)**

Verifying - Enter PEM pass phrase: **(entrez le même mot de passe)**

-----

You are about to be asked to enter information that will be incorporated

into your certificate request.

What you are about to enter is what is called a Distinguished Name or a
DN.

There are quite a few fields but you can leave some blank

For some fields there will be a default value,

If you enter '.', the field will be left blank.

-----

Country Name (2 letter code) \[AU\]:**FR (entrez un pays, important
uniquement pour les certificats commerciaux incorporant vos informations
de société, adresse etc.)**

State or Province Name (full name) \[Some-State\]:**Ile-de-France **

Locality Name (eg, city) \[\]:**Paris **

Organization Name (eg, company) \[Internet Widgits Pty Ltd\]:**AlternC
**

Organizational Unit Name (eg, section) \[\]:**www **

Common Name (eg, YOUR name) \[\]:**demo.alternc.org (Cette information
EST VITALE : elle déterminera le nom de domaine qui sera signé par
l'autorité de certification. Ne vous trompez pas : mettez bien l'adresse
DNS qualifiée de votre serveur.)**

Email Address \[\]:**root@alternc.org (mettez votre email ici, cette
information ne sera pas utilisée, sauf à la demande d'un certificat
d'email, ce qui n'est pas le cas ici)**

Please enter the following 'extra' attributes

to be sent with your certificate request

A challenge password \[\]: **(n'entrez rien, juste &lt;enter&gt;)**

An optional company name \[\]: **(n'entrez rien, juste &lt;enter&gt;)**

-----BEGIN CERTIFICATE REQUEST-----

MIIB0jCCATsCAQAwgZExCzAJBgNVBAYTAkZSMRYwFAYDVQQIEw1JbGUtZGUtRnJh

bmNlMQ4wDAYDVQQHEwVQYXJpczEQMA4GA1UEChMHQWx0ZXJuQzEMMAoGA1UECxMD

d3d3MRkwFwYDVQQDExBkZW1vLmFsdGVybmMub3JnMR8wHQYJKoZIhvcNAQkBFhBy

b290QGFsdGVybmMub3JnMIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDBBc3t

MX4/RwUT7hcksOLUCpWh3B5vRUbjgyFzxqxWRz2MHFAOBkvR+7TsYSL03EOdJ8hk

imyUlSvSGaQdgyysHCx47YvI81yRsFRAk6KAl/nc2Zi7H+aMZeroYBDhMbb+OZNb

3jxvqDVUIsSyLm5fFqsfl5W+AakLKmEBkaR+qQIDAQABoAAwDQYJKoZIhvcNAQEF

BQADgYEAfzbUzl171L+hCshrPpI5a9vPT3RPTh42dIgA106NJsGTYcIZHpnfw8+l

5etqEztnLCwVb/s7w+bb8a1l79CDwLlTmDNZjVGrp1yeh1j14kwxlnK+gHlSsDJk

NhOTQg+/WC+pIXqRHImvLfdCN9wMzz0MtkJCErWdByLdxKHO/A8=

-----END CERTIFICATE REQUEST-----

Voilà ! Votre clé privée protégée par un mot de passe a été sauvée dans
un fichier *privkey.pem*, et votre requête de certificat a été affichée
sur la console comme ci-dessus.

Nous allons déjà sauver dans un fichier votre CSR en tapant :

----------------------------------
### instruction certificat csr alternc

sudo cat alternc.csr |xargs &gt; &gt; alternc.csr.BAK

----------------------------------

collez dans la console votre CSR (le Certificate Request ci-dessus, y
compris les lignes avec ----)

puis taper &lt;Enter&gt; et &lt;Control-d&gt; pour quitter *cat*.

Ensuite, nous allons déchiffrer la clé privée RSA pour la mettre dans le
fichier qu'utilisera AlternC pour configurer votre serveur. taper :

----------------------------------
### instruction (N°1) certificat pem apache2

sudo openssl rsa -in privkey.pem -out apache.pem

----------------------------------

Votre mot de passe temporaire utilisé plus haut sera requis. taper-le
puis &lt;enter&gt; :

Enter pass phrase for privkey.pem:

writing RSA key

Ensuite, rendez-vous sur le site de votre autorité de certification, et
commencez la génération du certificat (appellée aussi 'enrollment' dans
ce petit monde de la certification).

À un moment de l'enrollement, l'autorité demandera votre requête de
certificat (le CSR). Copiez donc le code de la Certificate Request plus
haut (y compris les lignes avec ----).

Ensuite, l'autorité délivrera votre certificat, soit à l'écran, soit
sous forme d'un fichier téléchargeable. Ce fichier contient un
certificat qui ressemble à celui montré au début de ce chapitre.

Collez ce certificat dans votre presse-papier et taper, en tant que
root, sur votre serveur :

----------------------------------
### instruction (n°2) certificat pem apache2

sudo cat alternc.csr.BAK |xargs &gt;&gt;/etc/alternc/apache.pem

----------------------------------

Collez dans ce cat votre certificat (y compris les lignes avec -----) et
taper Enter et Control-d pour quitter cat.

Vous avez ajouté ainsi à la fin du fichier apache.pem votre certificat.
Ce fichier contient donc désormais les clés privées et certificat de
votre serveur, l'un sous l'autre.

Dernière étape : l'installation de votre certificat dans les logiciels
qui l'utiliseront. Pour cela, lancez tout simplement

----------------------------------
### instruction pour relancer AlternC

\#sudo alternc.install

----------------------------------

qui relance la configuration des services d'AlternC. cet installeur se
rend compte que le certificat a été installé dans apache.pem et
configure les logiciels comme il faut (courier-pop, courier-imap,
apache, postfix, proftpd) :

![img](images/1000000000000302000002004B3A6675.png)


La ligne qui vous le confirme (surlignée en noir dans la console
ci-dessus) est celle d'Apache indiquant « enabling module ssl »

![img](images/10000201000001DA000000940527E11E.png)

### Vérifier que votre configuration SSL soit bonne

Une fois tout cela fait, vous voudrez logiquement vérifier que tout cela
marche bien. Le plus simple est d'utiliser openssl sur une machine Linux
distante (typiquement la votre en local, pas sur votre serveur).

Si vous n'avez pas de machine Linux sous la main de disponible :

- un Mac dans une fenêtre de Terminal fera l'affaire. 
- Sous Microsoft Windows 10 vous pouvez installer [MobaXterm](https://mobaxterm.mobatek.net/)
- avec Cygwin (windows) qui contient le package openssl et vous permet
de lancer toutes les commandes associées.

-------------------------------------------------------

(Sous Cygwin ou MacOsX, assurez-vous de disposer d'un dossier avec les
certificats des autorités de certification reconnues, s'il n'est pas
/etc/ssl/certs, remplacez-le dans les lignes ci-dessous).

-------------------------------------------------------

Pour vérifier le certificat d'un service, nous allons utiliser la
commande *openssl s_client*, qui permet de simuler un client ssl ou
starttls. Cette commande va vous vérifier le certificat fourni par le
serveur en SSL ou via la commande STARTTLS. Remplacez bien évidemment
demo.alternc.org par l'adresse de votre serveur.

### Vérifier le SMTP :

-------------------------------------------------------
#### instructions Openssl ( *imap-ssl* et *pop-ssl*)

sudo openssl s_client -CApath /etc/ssl/certs -connect demo.alternc.org:25
-starttls smtp


sudo openssl s_client -CApath /etc/ssl/certs -connect demo.alternc.org:993

sudo openssl s_client -CApath /etc/ssl/certs -connect demo.alternc.org:995

#### https :

sudo openssl s_client -CApath /etc/ssl/certs -connect demo.alternc.org:443

-------------------------------------------------------

À chaque fois, la commande se connecte à votre serveur et vérifie la
validité de votre certificat par rapport aux autorités locales
installées sur le PC qui contrôle.

OpenSSL doit donc afficher quelque chose comme ceci :

New, TLSv1/SSLv3, Cipher is DHE-RSA-AES256-SHA

Server public key is 2048 bit

Compression: NONE

Expansion: NONE

SSL-Session:

Protocol : TLSv1

Cipher : DHE-RSA-AES256-SHA

Session-ID:
BA14AC86177FADBED803325BF91BE81BABA7C2441F9217A86A837BC2EE825AA9

Session-ID-ctx:

Master-Key:
6A42787B38AB012C4284987C5BA21208F286D4B9990B9DAEC75B68120C2A3DAE8C31072D96CBFCF214EBD37C9E38ECAA

Key-Arg : None

Start Time: 1277661761

Timeout : 300 (sec)

Verify return code: 0 (ok)

---

La ligne importante est « Verify return code » qui doit valoir « 0
(ok) ». Sinon votre certificat est probablement mal installé.

Services externes
-----------------

Pour chaque nom de domaine hébergé sur votre serveur, il faut préciser
au moins 2 serveurs DNS qui répondront aux requêtes DNS des internautes
pour le domaines installé.

Votre serveur sous AlternC peut servir de DNS primaire pour vos domaines
installés, mais il lui faut un DNS secondaire, appelé aussi esclave DNS.

De même, votre serveur sous AlternC héberge des comptes email pop ou
imap, ou des alias, pour lesquels il peut être utile de disposer d'un
serveur de secours, appelé MX secondaire. Celui ci recevra le courrier
si votre serveur est en panne ou éteint. Le serveur MX secondaire
renverra en bloc votre courrier quand votre serveur AlternC principal
sera à nouveau en fonction.

Ces 2 services, qui nécessitent un second serveur, peuvent être
facilement configurés via AlternC, et voici comment ...


------------------------------------------------------------------------------

------------------------------------------------------------------------------

### DNS Esclave sur un autre serveur

Le principe du DNS esclave, ou DNS secondaire, est assez simple : votre
serveur, qui héberge un certain nombre de domaines, dispose d'un fichier
zone (situé dans /var/alternc/bind/zones/) pour chaque domaine hébergé.

Pour disposer d'un DNS secondaire, il faut réaliser deux tâches :

\- le serveur secondaire doit connaître la liste des noms de domaines
que vous hébergez,

\- il doit synchroniser le contenu de la zone de chacun de ces domaines,
pour pouvoir servir les requêtes DNS indépendamment de l'état de
fonctionnement du DNS primaire.

La seconde tâche est exécutée naturellement par les serveurs DNS comme
Bind9, que nous utilisons sur AlternC et que nous conseillons. Le
protocole DNS définit pour cela une commande spéciale, appelée AXFR, ou
« Transfert de Zone ».

### En voici un exemple :

------------------------------------------------------------------------------

apollon:~\# dig axfr alternc.org @primary

; &lt;&lt;&gt;&gt; DiG 9.5.1-P3 &lt;&lt;&gt;&gt; axfr alternc.org
@primary

;; global options: printcmd

alternc.org.14400INSOAprimary.heberge.info. root.brassens.heberge.info.
2010020102 2160 360 60480 14400

alternc.org.14400INNSprimary.heberge.info.

alternc.org.14400INNSsecondary.heberge.info.

alternc.org.14400INMX5 alternc.org.

alternc.org.14400INA91.194.60.27

debian.alternc.org.14400INA91.194.60.27

demo.alternc.org.14400INA91.194.60.151

svn.alternc.org.14400INA91.194.60.27

www.alternc.org.14400INA91.194.60.27

;; Query time: 2 msec

;; SERVER: 91.194.60.2\#53(91.194.60.2)

;; WHEN: Fri Oct 1 10:30:55 2010

------------------------------------------------------------------------------


------------------------------------------------------------------------------


La première tâche, elle, était traditionnellement faite « à la main » :
un administrateur système demandait à un autre de lui servir de DNS
secondaire pour une liste de domaines donnée, et les domaines étaient
configurés sur le secondaire à la main.

Avec AlternC, il devient nécessaire d'automatiser cette tâche, car
n'importe quel utilisateur du panel peut ajouter ou supprimer un ou
plusieurs noms de domaine sur son compte.

Il existe donc un package debian, nommé « alternc-slavedns », qui doit
être installé sur un serveur DNS secondaire (qui n'utilise lui-même pas
forcément AlternC), et qui permet au secondaire de synchroniser la liste
des domaines installés avec un ou plusieurs serveurs sous AlternC.

------------------------------------------------------------------------------

Voici comment installer ce package sur un serveur debian :

- ajouter tout d'abord le dépôt debian de AlternC sur le serveur

-------------------------------------------------------
#### code sources.list

sudo cat >/etc/apt/sources.list.d/alternc.list<<EOF

deb http://debian.alternc.org stable main

\#wget -cqO - https://debian.alternc.org/key.txt |sudo apt-key add -

EOF

### alternative :

echo "deb http://debian.alternc.org stable main" | sudo tee /etc/apt/sources.list.d/alternc.list

-------------------------------------------------------

- Utliser la commande wget et apt :

------------------------------------------------------------------------------

#### instruction :

sudo apt install wget

wget -cqO - https://debian.alternc.org/key.txt |sudo apt-key add -

sudo apt update

-----------------------------------------------------------------------------

-----------------------------------------------------------------------------

Le package alternc-slavedns, ainsi que ses dépendances (essentiellement
wget et bind9) seront installés en même temps.

Une fois ce package installé, vous devrez dérouler la procédure suivante
pour chaque serveur AlternC utilisant ce serveur secondaire :

\- Sur votre serveur AlternC, connectez-vous sur le compte « admin » et
cliquez sur « panneau administrateur »

![img](images/10000000000002B5000001456A940ED2.png)

Cliquez ensuite sur « Gérer les IP autorisées à faire des transferts de
zone esclave DNS ».

Dans ce formulaire, vous devez renseigner l'adresse IP du ou des
serveurs DNS secondaires qui se connecteront à votre serveur sous
AlternC. Dans notre exemple, nous avons mis 91.194.60.2 et
91.194.60.99 :

![img](images/100000000000030A000001A24E36A8B7.png)

Cette autorisation sera injectée dans la configuration de Bind dans
l'heure qui suit, et Bind autorisera alors ces adresses IP, et
uniquement celles-ci, à utiliser la requête DNS « AXFR » sur les
domaines hébergés.

Ensuite, revenez sur le « panneau administrateur » et cliquez sur
« Gérer les comptes autorisés pour les transferts de zone esclave DNS ».

Dans ce formulaire, créez un compte pour chaque serveur DNS secondaire
qui se synchronisera sur votre serveur AlternC. Ici nous avons créé 2
comptes, un pour primary et un pour secondary, qui sont nos 2 serveurs
DNS principaux (voir les architectures proposées plus bas dans ce
chapitre) :

![img](images/10000000000002EE0000018B0D16B823.png)

Voilà. Attendez une petite heure, et connectez-vous alors sur votre
serveur DNS secondaire pour y configurer votre serveur sous AlternC
comme suit (dans notre exemple, le secondaire s'appelle « secondary » et
le serveur AlternC s'appelle « alternc » :


\- Recopiez le fichier defaults.conf sous un autre nom (typiquement
celui de votre serveur AlternC) :

-------------------------------------------------------
#### code slavedns

 sudo cd /etc/alternc/slavedns/
 sudo cp defaults.conf alternc.conf

-------------------------------------------------------
\- modifier le fichier alternc.conf et remplacer les
informations requises :

-------------------------------------------------------
##### instructions pour remplacer dans alternc.conf

sudo sensible-editeur alternc.conf

PROTOCOL=http s

HOST=alternc.octopuce.fr

MASTERIP=91.194.60.152

LOGIN=secondary

PASSWORD=xahqu0Le

-------------------------------------------------------

#### explications des modifications du fichier de configuration:

- *protocol* : mettre HTTP si le serveur AlternC n'a aucun certificat SSL.
Sinon mettez HTTPS

- *host* : remplacer le nom de domaine du serveur alternc, ici 
« alternc.octopuce.fr »

- *masterip* : mettre l'adresse IP publique du serveur AlternC.

- *login* : remplacer le nom d'utilisateur (ici : secondary )

- *password* : remplacer le mot de passe (ici : xahqu0Le)

-------------------------------------------------------

c'est tout !

Désormais, toutes les heures, votre serveurDNS secondaire ira chercher
la liste des domaines hébergés sur votre ou vos serveurs sous AlternC,
et modifiera automatiquement la configuration de son bind9 pour lui
demander de synchroniser le contenu des zones DNS depuis les serveurs
AlternC

Comment cela marche-t-il ?

Pour mettre le nez dans le moteur, lancez, sur votre serveur secondaire,
la commande suivante :

-------------------------------------------------------
#### instruction (n°1) slavedns (en tant que root ou sudoer)

\#sudo alternc-slavedns

-------------------------------------------------------

no change found for 'alternc.conf'

Pour fonctionner, alternc-slavedns télécharge une page sur votre
AlternC, en s'identifiant avec le login et le mot de passe que vous avez
défini dans le panneau de contrôle administrateur.

Si de nouveaux domaines ont été installés ou si des domaines ont été
désinstallés du serveur AlternC correspondant, le fichier reçu est
transformé en fichier de configuration valide pour bind9, et bind9 est
rechargé. Les fichiers de alternc-slavedns sont situés à 2 endroits :

\- la liste des domaines de chaque serveur est dans
/var/cache/slavedns/, avec tout simplement un fichier .conf par serveur
AlternC, et une ligne par domaine hébergé

-------------------------------------------------------
#### instruction (n°2) slavedns (en tant que root ou sudoer)

\#sudo cat /var/cache/slavedns/brassens.conf


alternc.org

alternc.fr

mondomaine.com

-------------------------------------------------------

\- la configuration de bind9 est stockée dans un fichier .conf
par serveur AlternC, et un bloc slave par domaine.


-------------------------------------------------------
#### exemple pour montest.fr :

sudo cat /etc/bind9/slavedns/montest.fr.conf

zone "montest.fr" {

type slave;

allow-query { any; };

file "montest.fr";

masters { 91.194.60.152; };

};

-------------------------------------------------------

Vous pouvez ainsi vérifier facilement que alternc-slavedns a bien pris
en compte vos nouveaux noms de domaines.

Si vous voulez forcer la mise à jour immédiate de la liste des
domaines :

-------------------------------------------------------

#### instruction (n°3) slavedns (en tant que root ou sudoer)

\# sudo alternc-slavedns

-------------------------------------------------------

changes detected for 'brassens.conf', applying

Comment architecturer mes serveurs ?
====================================

La question est intéressante et nous a souvent été posée. Lorsque vous
disposez de plusieurs serveurs sous AlternC, il est évident que vous
pouvez utiliser un seul et même secondaire qui se synchronisera avec
tous vos serveurs.

Par exemple, si vous avez 10 serveurs AlternC numérotés de
alternc1.example.com à alternc10.example.com, vous pouvez très bien
installer alternc-slavedns sur alternc1 et 2, configurer le 1 pour qu'il
se synchronise sur le alternc-slavedns du 2, et configurer tous les
autres pour qu'ils se synchronisent sur le 1.

Ainsi, vos domaines hébergés sur alterncX indiqueront chez le registrar
les serveurs DNS :

alternc1.example.com.

alterncX.example.com.

pour qu'ils soient hébergés par votre infrastructure, sauf les domaines
de alternc1 qui mettront alternc1 et alternc2.

Cette solution parait facile à mettre en œuvre, mais peut porter à
confusion car elle utilise le même nom pour le panneau de contrôle web
et le serveur DNS.

On préfèrera donc utiliser un second nom DNS pour vos 2 serveurs, et
installer AlternC-slavedns sur 2 d'entre eux.

Ainsi, faites pointer, par exemple, le nom primary.example.com vers l'IP
de alternc1 et secondary.example.com vers l'IP de alternc2

Installez ensuite alternc-slavedns sur alternc1 et alternc2 et
configurez tous vos serveurs pour qu'ils se synchronisent sur alternc1
et alternc2 (sauf sur eux-même bien entendu)

Enfin, demandez à tous vos utilisateurs de mettre comme serveurs DNS,
chez leur registrar, les valeurs suivantes :

primary.example.com.

secondary.example.com.

Cette configuration est plus simple à mémoriser. Elle permet
éventuellement, à terme, de disposer de serveurs DNS en dehors de votre
réseau (externaliser le secondary) ou de déplacer ce service facilement
sur d'autres serveurs.

Et si je n'ai qu'un seul serveur sous AlternC avec une seule IP ?

C'est un cas classique des toutes petites structures qui hébergent
elles-même un serveur chez un gros prestataire (comme rackspace ou ovh),
ou qui hébergent sur leur accès ADSL et qui souhaitent disposer d'un
serveur DNS secondaire automatique.

L'équipe d'AlternC fournit gracieusement un service de DNS secondaire
pour ces utilisateurs : pour cela, envoyez un simple email à
equipe@alternc.org ou contactez-nous sur l'irc \#alternc sur freenode,
et envoyez-nous les informations suivantes :

\- le nom dns de votre serveur AlternC

\- son adresse IP publique

\- le login et le mot de passe à utiliser pour synchroniser la liste des
DNS

De votre côté, vous devrez autoriser l'IP 91.194.60.145 à effectuer un
transfert de zone DNS, et vous devrez mettre comme serveur DNS chez
votre registrar :

\- votre serveur comme primaire

\- ns2.alternc.org comme secondaire

voilà, c'est aussi simple que cela !

### MX Secondaire sur un autre serveur

Le principe du MX secondaire, appellé aussi « backup MX », est simple :
votre serveur héberge un certain nombre de domaines. Il dispose d'un
fichier zone (situé dans /var/alternc/bind/zones/). pour chaqcun de ces
domaines. Ce fichier précise quel serveur héberge le mail de ce domaine,
sous la forme d'une ligne, nommée « champ MX » comme dans l'exemple
ci-dessous :

$TTL 4H

;

; BIND data file for domain alternc.org

;

@ IN SOA primary.heberge.info. root.brassens.heberge.info. (

2010020102 ; serial

2160 ; refresh (6h)

360 ; retry (1h)

60480 ; expiry (7d)

14400 ) ; RR TTL (24h)

IN NS primary.heberge.info.

IN NS secondary.heberge.info.

**IN MX 5 brassens.heberge.info.**

IN A 91.194.60.27

La ligne avec le « MX 5 » montre donc que « brassens.heberge.info » est
le serveur de mail du domaine alternc.org.

Hélas, un serveur étant par définition peu fiable, il arrive qu'il tombe
en panne, et pendant ce temps, votre mail n'arrive pas, et les autres
serveurs SMTP du monde entier qui tentent de s'adresser à vous doivent
conserver les mails qui vous sont destinés. Une fois votre serveur de
mail de retour, il peut se passer un temps important avant que vous ayez
reçu tous les mails en attente, et vous pourriez même en avoir perdu
certains, si les serveurs d'en face n'ont pas conservé assez longtemps
les mails de votre domaine.

C'est pour cette bonne raison qu'il est utile de disposer d'un second
serveur de mail, qui apparait alors sous la forme d'une seconde ligne IN
MX 10 (le 10 étant un poids plus faible, ce n'est pas lui qui recevra
les mails en priorité) :

IN NS primary.heberge.info.

IN NS secondary.heberge.info.

**IN MX 5 brassens.heberge.info.**

\*\* IN MX 10 apollon.serverside.fr.\*\*

IN A 91.194.60.27

En cas de défaillance de *brassens*, *apollon* réceptionnera les méls
et les réexpédiera à *brassens* après remise en service de celui-ci.

Mettre une ligne de MX secondaire au serveur AlternC de tous les domaines hébergés. Modifier le fichier exemple (ci-dessous).

----------------------------------------------------------------

#### exemple pour /etc/alternc/templates/bind/templates/zone.template 

$TTL 1D

;

; BIND data file for domain @@DOMAINE@@

;

@ IN SOA %%ns1%%. root.%%fqdn%%. (

@@SERIAL@@ ; serial

21600 ; refresh (6h)

3600 ; retry (1h)

604800 ; expiry (7d)

86400 ) ; RR TTL (24h)

IN NS %%ns1%%.

IN NS %%ns2%%.

IN MX 5 %%mx%%.

IN MX 10 apollon.serverside.fr.

----------------------------------------------------------------

-----------------------------------------------------------------

#### instruction pour réactualiser AlternC

sudo alternc.install

-----------------------------------------------------------------

#### **Remarque** :

- Relancer **alternc.install** pour propager le modèle dans 
/etc/bind/templates/

- Il est possible de modifier directement zone.template (ci-dessous)

-----------------------------------------------------------------
#### Modification MX 10 directement applicable

sudo echo "IN MX 10 apollon.serverside.fr." >>/etc/bind/templates/zone.template

-----------------------------------------------------------------

**Attention **: Sur un serveur disposant déjà de domaines installés, il
faudra ajouter cette ligne MX à la main dans les fichiers de zone déjà
créés dans /var/alternc/bind/zones/. N'oubliez pas, dans ce cas, de
modifier la valeur du nombre « serial » pour que la zone soit bien prise
en compte partout dans le monde !

Ensuite, nous allons effectuer les mêmes manipulations pour la
déclaration des domaines hébergés sur le serveur que dans le chapitre
précédent traitant des DNS secondaire.

Pour disposer d'un MX secondaire, il faut que ce serveur secondaire
puisse connaître la liste des noms de domaines que vous hébergez,

Il existe donc un package debian, nommé « alternc-secondarymx », qui
doit être installé sur un serveur MX secondaire (qui n'utilise lui-même
pas forcément AlternC), et qui permet au secondaire de synchroniser la
liste des domaines installés avec un ou plusieurs serveurs sous AlternC.

Ajouter le dépôt et installer le paquet (comme ci-dessous)

-----------------------------------------------------------------
#### Code pour un serveur debian secondaire

\# echo "deb http://debian.alternc.org stable main" | sudo tee /etc/apt/sources.list.d/alternc.list

\# sudo apt install wget

\# wget -cqO - https://debian.alternc.org/key.txt |sudo apt-key add -

\# sudo apt update

\# sudo apt install alternc-secondarymx

-----------------------------------------------------------------

\- Installer simultanément le paquet alternc-secondarymx et ses   dépendances (essentiellement postfix).

\- dérouler la procédure pour chaque serveur AlternC utilisant
  ce serveur secondaire.

\- Sur votre serveur AlternC, connectez-vous sur le compte « admin » et
cliquez sur « panneau administrateur »

![img](images/10000000000002B5000001456A940ED2.png)

- Cliquer sur « Gérer les comptes autorisés pour les mx secondaires ».

- Créer un compte pour chaque serveur MX secondaire qui se
  synchronisera sur votre serveur AlternC. Création d'un compte 
  pour *apollon* (cf illustration ci-dessous)

![img](images/100000000000030A0000015D9BBB9E9B.png)

- Connecter vous sur le serveur MX secondaire (dénommé *apollon*) et
  configurer votre serveur sous AlternC (dénommé *alternc*)
  
\- Allez dans (exemple ci-dessous).

------------------------------------------------------------------

###### instructions

\# sudo cd /etc/secondarymx/

\# sudo cp secondarymx.conf alternc.conf

------------------------------------------------------------------


------------------------------------------------------------------

### Modifications du fichier alternc.conf (remplacez les informations)

\#sudo sensible-editor alternc.conf

HOST=alternc.octopuce.fr

MASTERIP=91.194.60.152

LOGIN=apollon

PASSWORD=yi0Ar4quae

------------------------------------------------------------------

#### explications des modifications du fichier de configuration

- *host* : mettre le nom de domaine du serveur alternc « alternc.octopuce.fr »

- *masterip* : renseigner l'adresse IP publique du serveur alternc.

- *login* : renseigner le nom d'utilisateur (ici : apollon ) précédemment
            créer sur le serveur AlternC

- *password* : renseigner le mot de passe (ici : yi0Ar4quae )
               précédemment créer sur le serveur AlternC

------------------------------------------------------------------

Désormais, toutes les heures, le serveur MX secondaire cherchera
la liste des domaines hébergés sur le ou les serveurs sous AlternC,
et modifiera automatiquement la configuration de postfix afin de
de servir de serveur de mail de secours pour vos serveurs AlternC

Comment cela marche-t-il ?

Mettre le nez dans le moteur, lancer, sur le serveur secondaire,
la commande suivante (instruction ci-dessous) :

------------------------------------------------------------------

#### instruction pour le MX secondaire

\# sudo secondarymx

*réponse* : no change found for 'alternc.conf'

------------------------------------------------------------------

En fonctionnement, alternc-secondarymx télécharge une page sur 
AlternC, en s'identifiant avec le login et le mot de passe ainsi défini dans le panneau de contrôle administrateur.

Si des nouveaux domaines ont été installés ou désinstallés du serveur AlternC correspondant, le fichier reçu est transformé en fichier de configuration valide pour recharger postfix.
Les fichiers de alternc-secondarymx sont situés dans
/var/cache/secondarymx.

Vous pouvez ainsi vérifier que alternc-slavedns prenne en compte les nouveaux noms de domaines.

Si vous souhaitez forcer immédiatement la mise à jour de la liste des
domaines sur le serveur secondaire :

------------------------------------------------------------------

#### instruction sur le MX secondaire

\# sudo alternc-secondarymx

------------------------------------------------------------------

Chapitre VIII - Trucs & Astuces pour AlternC
============================================

Introduction
------------

Ce chapitre va vous donner un ensemble de trucs et astuces pour
l'administrateur système utilisant AlternC. La plupart des astuces
décrites ici nécessitent des bases en administration système.

Comme bien souvent dans les logiciels libres développés par la
communauté, si vous avez d'autres questions, vous pouvez utiliser les
listes de discussions du projet pour les poser, après avoir cherché les
archives de ces listes bien entendu !

Le projet AlternC dispose de 2 listes de discussion à ce jour :

**user@alternc.org**, qui est la liste de discussion des administrateurs
systèmes utilisateurs d'AlternC. Si vous avez un serveur sous AlternC,
nous vous conseillons vivement de vous y inscrire : il y a assez peu de
message, mais ils sont souvent pertinents : annonce de nouvelles
versions et questions d'autres utilisateurs vous aideront à approfondir
votre connaissance de Debian, de Linux et d'AlternC en particulier.

**dev@alternc.org**, qui concerne surtout ceux qui développent AlternC.
C'est le meilleur endroit pour poser une questions véritablement pointue
et nécessitant des compétences importantes. Si vous souhaitez aider au
développement d'AlternC, c'est un endroit quasiment incontournable pour
bien suivre le projet.

Nous espérons enfin que vos premières questions trouveront réponse dans
les trucs & astuces présentes ici même. C'est parti !

Trucs & Astuces
---------------

### Emplacement des données dans AlternC

***Question *: Où se trouvent les données web et mail des utilisateurs ?
**

Dans un serveur utilisant AlternC, les emplacements par défaut des
données sont les suivants :

Les données web des comptes AlternC sont dans /var/alternc/html/, ou
plus précisément :

/var/alternc/html/&lt;premiere lettre du compte&gt;/&lt;nom du
compte&gt;

Les données des adresses emails des utilisateurs sont, quant à elles,
dans :

/var/alternc/mail/&lt;premiere lettre du compte POP/IMAP&gt;/&lt;nom du
compte POP/IMAP&gt;

où le « nom du compte pop/imap » est l'adresse email ou @ est remplacé
par \_.

Si vous souhaiter pouvoir accéder facilement aux données HTML de vos
comptes AlternC, vous pouvez, en tant que root, créer un lien symbolique
de /var/alternc/html à la racine :

--------------------------------------------------------------------

#### instruction lien

ln -sf /var/alternc/html /html

--------------------------------------------------------------------
ainsi, vous pouvez accéder facilement par exemple au compte demo ainsi :

cd /html/d/demo

### Mailman

*Question *: Où se trouvent les listes de diffusion Mailman ?

Comme pour tout mailman standard de Debian GNU/Linux, Les listes mailman
gérées par AlternC sont dans

/var/lib/mailman/lists/

Dans le dossier « lists » vous trouverez un dossier par liste dans
lequel se trouvent les fichiers de configuration de mailman (config.pck)
et d'autres données (par exemple les pages HTML personnalisées)

***Question *: Où se trouvent les archives des listes ?**

Les archives des listes mailman se situent dans le dossier

/var/lib/mailman/archives/

Dans ce dossier se trouvent 2 sous-dossiers : public et private.

Public contient uniquement des liens symboliques vers les archives dans
private/, pour les listes dont les archives ont été déclarées publiques.

Dans private, on trouve 2 dossiers par liste :

\- un au nom de la liste, qui contient les archives au format HTML

\- un au « nom de la liste.mbox », qui contient un fichier mailbox :
c'est la liste des emails reçus sur cette liste, lisibles avec mutt.

Étant donné que depuis l'interface mailman il n'est pas possible de
supprimer des archives, vous pouvez procéder à leur suppression en
console par exemple.

***Question :*** **Comment régénérer les archives des listes mailman ?**

La régénération se fait, pour une liste, de la manière suivante :

se connecter en root dans le répertoire indiqué ci-dessous et déclencher
la régénération des archives

----------------------------------------------------------------------
#### instruction régénération liste mailman

\# cd /var/lib/mailman/bin

\# ./arch nomdelaliste

----------------------------------------------------------------------

*Attention*, si vous souhaitez régénérer l'ensemble des archives d'une
liste, effacez tout d'abord les archives HTML dans

/var/lib/mailman/archives/private

Question : Comment enlever un email des archives d'une liste mailman ?

Pour régénérer ou modifier les archives d'une liste, le plus simple est
de procéder ainsi (par exemple pour la liste « discussion ») :

- Vérifier l'installation de mutt :

----------------------------------------------------------------------
#### Code install mutt

sudo apt install mutt

----------------------------------------------------------------------

- Supprimer les archives HTML de la liste :

----------------------------------------------------------------------
#### instructions remove file Mailman

\# sudo rm -rf /var/lib/mailman/archives/private/discussion/\*

----------------------------------------------------------------------

- Se rendre dans le second dossier d'archive et lancez Mutt :

----------------------------------------------------------------------
#### instructions mutt

\#sudo cd /var/lib/mailman/archives/private/discussion.mbox/

mutt -f discussion.mbox

----------------------------------------------------------------------

- Avec Mutt, rechercher et supprimer individuellement les messages en appuyant sur « d » et valider avec avec la touche « $ » , pour quitter Mutt appuyer sur la touche « q ».

- Regénérer les archives publiques (ou privées) par la commande arch :

----------------------------------------------------------------------
#### instructions (n°1) Liste Mailman

sudo su  list

cd /var/lib/mailman/bin/

./arch discussion

----------------------------------------------------------------------

- *Remarque* : la régénération peut durer un certain temps selon le
               volume des archives.

***Question : ***Comment disposer d'un mot de passe valide pour toutes
les listes ?

Mailman dispose d'une fonctionnalité intéressante pour les
administrateurs systèmes, à savoir le fait de disposer d'un « mot de
passe partout ». Pour fixer ou modifier ce mot de passe, on procède
ainsi :

----------------------------------------------------------------------
#### instructions (n°2) Liste Mailman

\# sudo cd /var/lib/mailman/bin

\# sudo ./mmsite

----------------------------------------------------------------------

- Valider deux fois votre nouveau mot de passe qui sera actif pour
  toutes les listes mailman.

### Mariadb

*Question *: Où se trouve le mot de passe du PhpMyAdmin général ?

- Suivre notre didacticiel d'installation d'AlternC et lancer l'instruction : 

----------------------------------------------------------------------
#### instruction mot de passe MySQL / Mariadb

\# sudo mysql_secure_installation

----------------------------------------------------------------------

#### obectif de l'instruction ci-dessus :

- création d'un mot de passe pour le compte root de Mariadb

- permettre un accès illimité à toutes les bases et tous les droits
  dans Mariadb

- pouvoir utiliser cet identifiant (login) et ce mot de passe et
  permettre de se connecter à Mariadb :
  
      * via l'interface générale ouaibe de PhpMyAdmin

	  * via la ligne de commande (ci-dessous)

----------------------------------------------------------------------
#### instruction mot de passe MySQL / Mariadb

\# mysql -u *root* -p *votremotdepasse*

----------------------------------------------------------------------

- Autrement pouvoir employer directement la commande « mysql », sous
  réserve de posséder un fichier .my.cnf contenant les bonnes
  informations dans votre compte.

- Création d'un lien symbolique du fichier .my.cnf pour Mariadb situé 
  dans :  /etc/alternc/my.cnf

----------------------------------------------------------------------
#### instruction lien MySQL / Mariadb

\# sudo  ln -sf /etc/alternc/my.cnf /root/.my.cnf

----------------------------------------------------------------------

- En étant root ou sudoer, taper la commande "mysql" permet de se 
  connecter automatiquement sans demander le mot de passe général.

----------------------------------------------------------------------
#### instruction commande MySQL / Mariadb

~#\ mysql &lt;enter&gt;

----------------------------------------------------------------------

### Awstats

*Question *: Comment regénérer les stats awstats ?

Awstats calcule tous les matins les statistiques de la veille pour tous
les sites web

Si vous devez recalculer les statistiques d'un site depuis sa création,
vous pouvez procéder ainsi :

\- dans /var/cache/awstats, trouvez le sous-dossier du domaine
correspondant (par exemple ici, mondomaine.fr)

serveur:/var/cache/awstats\# ls

mondomaine.fr

intranet.unautre.com

www.lulu.net

Rendez-vous dans ce sous-dossier et déplacez dans un dossier de
sauvegarde tous les fichiers présents dans ce sous-dossier (par exemple
un sous dossier du répertoire root)

mkdir /root/mondomaine

mv \* /root/mondomaine/

lancez ensuite la commande suivante en tant que www-data :

\# sudo su www-data

$ /usr/lib/alternc/alternc-awstats mondomaine.fr

Cela relancera les stats de mondomaine.fr sur toute la période couverte
par vos logs apache2, et pour les mois où vous avez retiré les fichiers
de cache.

Si vous voulez le faire pour tous vos domaines hébergés, vous pouvez
lancez plus généralement :

\# sudo su www-data

$ /usr/lib/alternc/alternc-awstats all

(attention, cela peut prendre vraiment beaucoup de temps sur une machine
chargée. N'hésitez pas à utiliser la commande « screen » pour lancer ce
process en arrière-plan)

Cela vous permet aussi de générer des statistiques pour un site qui
était hébergé depuis longtemps sur votre serveur.

### Divers

***Question *: un logiciel en PHP que j'installe sur le serveur
nécessite de désactiver le Safe Mode, et refuse de fonctionner sans.
Comment faire ?**

***Ou encore  *: j'ai écris ou j'utilise un programme qui nécessite de
lancer des exécutables sur le serveur, et nécessite de pouvoir accéder à
des dossiers en dehors de mon espace web. Comment faire ?**

Par défaut Safe Mode est à « on » sur Alternc. Ce paramètre de PHP est
une sécurité pour un serveur mutualisé. Il permet d'interdire certaines
commandes comme l'exécution de toute commande sur le serveur.

Par ailleurs, chaque sous-domaine est cloisonné à son espace web, par la
directive open\_basedir de php. Ainsi, dans
*/var/alternc/apacheconf/m/mondomaine.fr/* on trouve un extrait de
fichier de configuration apache qui emprisonne le site dans son espace
web, par exemple */var/alternc/html/u/uncompte/*. Ainsi chacun est
cloisonné chez soi.

Il peut arriver que vous ayez besoin de mettre Safe Mode à « off » pour
un domaine ou sous-domaine en particulier. En effet :

\- certains programmes en PHP nécessitent la désactivation du safe\_mode

\- certains programmes PHP ont besoin de pouvoir lancer des commandes
shell (comme imagemagick, latex ou d'autres commandes...)

Voici comment procéder pour un nom de domaine spécifique :

Créer un fichier dans /etc/apache2/conf.d/ au nom de votre domaine
(histoire de ne pas être perdu) et terminé par « .conf »

par exemple :

serveur:~/\# cd /etc/apache2/conf.d

serveur:/etc/apache2/conf.d\# vim www.nomdedomaine.tld.conf

Inscrire dans le fichier les lignes suivantes :

&lt;directory /var/alternc/dns/n/www.nomdedomaine.tld&gt;

php\_admin\_flag safe\_mode off

php\_admin\_flag safe\_mode\_gid off

&lt;/directory&gt;

Ensuite, sauvez le fichier ainsi créé et relancez apache comme suit :

\# sudo service apache2 reload

\# sudo service apache2 status

*Question :* Je viens d'installer un domaine sur mon serveur,

*ou *: Je viens de supprimer un domaine sur mon serveur,

*ou encore *: Je viens de changer la configuration d'un domaine sur mon
serveur,

...mais je ne veux pas attendre 5 minutes, comment faire ?

L'installation des domaines étant une opération nécessitant des droits
avancés, elle ne peut pas être réalisée par le panel, qui n'a pas de
droits particuliers.

Cette installation des domaines est donc réalisée par une tâche
planifiée lancée en tant qu'administrateur du serveur (root). Si vous
avez accès au serveur via SSH sur le compte root, vous pouvez lancer la
tâche planifiée quand vous le voulez. Pour cela, taper :

\#sudo  /usr/lib/alternc/update\_domains.sh

Cette commande doit rendre la main assez rapidement, les modifications
en attente sur les domaines ont toutes été prises en compte.

Question : Un des services du serveur est arrêté, comment puis-je le
relancer ?

à l'aide de systemd et de la commande service

avec comme paramètre stop, start, restart ou reload selon l'action
à réaliser.

Ainsi, il est possible de redémarrer ProFTPd avec :

#\ sudo service proftpd restart &lt;enter&gt;

#\ sudo service proftpd status &lt;enter&gt;

Pour recharger apache2 sans le redémarrer :

#\ sudo service apache2 reload &lt;enter&gt;

#\ sudo service apache2 status &lt;enter&gt;

#\ sudo journalctl -xep err &lt;enter&gt;


Pour arrêter postfix :

#\ sudo service postfix stop &lt;enter&gt;

#\ sudo service postfix status  &lt;enter&gt;

*Question *: mon site web me sort une page : « Internal Server Error »

Ce message d'erreur peut signifier 1000 choses différentes. Les plus
classiques étant :

* vous avez mis un fichier nommé « .htaccess » dans votre espace
    d'hébergement, et ce fichier, qui est un extrait de fichier de
    configuration d'apache, doit respecter une syntaxe très précise, et
    ne contenir aucune faute, sans quoi, votre site sera entièrement
    indisponible avec l'erreur ci-dessus.
* vous utilisez un cgi-bin et celui-ci échoue.

Dans tous les cas, apache mémorise l'erreur « 500 Internal Server
Error » dans son fichier de log. Taper en console :

#\ sudo tail -f /var/log/apache2/error.log &lt;enter&gt;

#\ sudo service apache2 status &lt;enter&gt;

#\ sudo journalctl -xep err &lt;enter&gt;

permet de voir défiler les nouveaux messages d'erreur de votre serveur
web. Rechargez la page provoquant l'erreur, et vous verrez la raison de
celle-ci, par exemple :

\[Thu Jun 24 12:46:47 2008\] \[alert\] \[client 91.194.61.194\]
/var/alternc/dns/m/www.mondomaine.tld/.htaccess: Illegal option

Cela montre dans quel fichier de configuration d'apache2 une erreur
s'est glissée, rendant le site inutilisable

*Question *: Pour l'installation d'un kit de paiement en ligne, je dois
poser un script CGI, comment faire ?

*ou *: J'ai un programme binaire compilé à installer sur le serveur pour
servir des pages CGI, comment faire ?

La plupart du temps, vous n'aurez pas besoin d'utiliser ce que l'on
appelle des cgi-bin (qui sont des programmes en Perl, C, ou tout autre
binaire, lancés par le serveur web à une adresse spécifique). Cependant,
pour certains usages, comme l'installation de kit de paiement en ligne,
vous aurez besoin d'installer un CGI-BIN de votre banque.

Les cgi-bin peuvent être installés à la main par root dans le dossier

/var/alternc/cgi-bin/

Dans ce dossier, vous pouvez mettre le programme à installer en tant que
cgi, et lui donner des droits d'exécution par la commande

chmod a+x votre-programme

Vous pouvez l'envoyez via sftp sur votre compte root.

Il sera accessible sur votre site à l'url

http://votredomaine.tld/cgi-bin/votre-programme

Vous pouvez aussi le mettre dans un sous-dossier de
*/var/alternc/cgi-bin/* auquel cas l'url deviendra

http://votredomaine.tld/cgi-bin/sous-dossier/votre-programme

Notez que les CGI ainsi installés sont accessibles à tout domaine
installé sur le serveur, à la même adresse. Ainsi, si vous avez
« unautredomaine.eu » sur votre serveur, l'url ci-dessous marchera
aussi :

http://unautredomaine.eu/cgi-bin/sous-dossier/votre-programme

***Question :***\*\* A l'affichage de mes pages les caractères accentués
sont incorrects\*\*

Sur certains sites, les accents peuvent apparaître ainsi :

jeu de caractÃ¨res par dÃ©faut au lieu de

jeu de caractères par défaut

Il se peut que cela vienne de la configuration par défaut d'Apache2, qui
est de fournir les pages comme étant encodées en ISO-8859-1.

Dans ce cas, essayez le truc suivant : à la racine de votre site, créez
un fichier nommé « .htaccess » et entrez la ligne suivante dans le
fichier (si ce fichier existe déjà, ajoutez cette ligne en bas du
fichier.)

AddDefaultCharset UTF-8

Cela passera toutes les pages de votre site en encodage UTF-8, et les
accents devraient désormais s'afficher correctement.

Si cela n'est pas résolu, il se peut que cela vienne de votre programme
PHP qui ne gère pas bien les jeux de caractères. Nous ne pouvons hélas
pas y faire grand chose : revoyez votre programme.

Subsidia

La neutralité du Net ne s'use que si on ne s'en sert pas !
==========================================================

Sébastien Canevet

décembre 2010

L'un des principes fondateurs d'internet conduit à traiter également
toute information qui circule sur le réseau, indépendamment de sa
source, de sa destination ou de son contenu. C'est la « neutralité du
net ».

Lors de la création d'internet, la rusticité des outils alors mis en
oeuvre ne permettait pas ce genre de discrimination. Aujourd'hui, le
perfectionnement des équipements techniques rend possible une gestion
discriminatoire du trafic. Cette idée malvenue ne manque pourtant pas de
partisans. Les arguments pour la combattre sont plutôt nombreux, de la
démocratie à la promotion de l'innovation et passant l'économie du
développement des infrastructures.

Loin des débats théoriques sur ces principes fondamentaux, certains
acteurs de l'internet ont choisi de participer à ce débat d'une autre
façon, en rendant rapidement et facilement accessible la mise en place
d'un hébergeur indépendant. Tel est le projet AlternC.

Alors que l'hébergement d'un site web était hors de portée des acteurs
autres que les opérateurs institutionnels, publics ou privés, la mise à
disposition de la suite logicielle AlternC permet depuis dix ans de
gérer aisément un hébergeur mutualisé, assurant ainsi une indépendance
technique à ceux qui souhaitent consacrer un minium de temps et quelques
moyens *(limités)* à cette tâche.

Projet libre, il était logique qu'AlternC fut également un logiciel
libre. A l'aube du projet, seuls les informaticiens *(ou presque)
*comprenaient les enjeux de ce choix. C'est donc en connaissance de
cause que les promoteurs du projet choisirent un logiciel libre
*(l'altern de Valentin Lacambre)* plutôt que son homologue propriétaire,
pour le faire évoluer.

C'est ainsi que, prouvant le mouvement en marchant, et mettant en oeuvre
la neutralité du net bien avant que la problématique n'héberge au sein
du public averti[4], AlternC permet la mise en oeuvre de l'hébergement
de sites internet librement, et ceci aux quatre sens du mot libre
lorsqu'il est accolé à nom logiciel.

* Première des quatre libertés, l' *« exécution du programme »*, ou
    plus largement l'utilisation d'AlternC est libre, en ce sens que
    vous pouvez utiliser ces logiciels pour n'importe quel usage,
    commercial ou pas, et ceci sans aucune limitation d'aucune sorte. Il
    est d'ailleurs important pour les logiciels libres en général que
    leur utilisation dans la sphère commerciale et industrielle soit la
    plus large possible, ce ci afin d'éviter qu'ils ne soient isolés
    dans un milieu étroit.…

* La liberté d'étude est la seconde liberté. Alors que leurs
    homologues\* « propriétaires »\* sont enfermés dans un carcan
    juridique étroit et compliqué[5], l'étude d'AlternC n'est soumise à
    aucune contrainte. C'est ainsi que l'on peut librement étudier le
    fonctionnement de ces logiciels, quel que soit l'objectif de cette
    étude. La libre disposition du code source facilite cette étude.

* La distribution de copie des logiciels AlternC est elle aussi
    possible librement, quelles que soient les conditions (gratuité ou
    pas...) de cette distribution. Une activité économique est ainsi
    parfaitement imaginable.

* Enfin, il est possible *(et même souhaitable)* qu'AlternC soit
    l'objet d'améliorations. Cette suite logicielle est une oeuvre *« in
    progress »*, en ce sens que, au sein de la communauté AlternC ou en
    dehors, toute personne ayant la compétence et la volonté de modifier
    ces programmes, peut le faire sans contrainte.

J'aime à répéter que ce que l'on ne trouve pas sur internet n'existe
pas, tant le réseau est aujourd'hui d'une richesse inouïe.

Bien au delà des seuls usages commerciaux, dans lesquels l'utilisateur
n'est envisagé que comme un consommateur, qui n'utiliserait le réseau
que pour dépenser son argent, se contentant pour le reste de demeurer
tranquillement sous perfusion télévisuelle, l'usager d'AlternC est un
véritable acteur du réseau. C'est pour cela que cet ouvrage a été écrit.

