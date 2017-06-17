
Installation de SSL pour AlternC
================================

AlternC gère pour l'instant un seul certificat SSL pour tout le serveur, qui sera utilisé pour le HTTPS via Apache2 du Panel, avec POP/IMAP dans Dovecot, avec SMTP dans Postfix, et avec FTP dans ProFtpd.

Pour cela, vous devez disposer d'un certificat X.509 valide, d'une clé privée, et si besoin (c'est le cas le plus souvent) d'un certificat chaîné vers votre autorité de certification (CA).

Nous vous recommandons d'utiliser [Letsencrypt](https://letsencrypt.org), autorité gratuite initiée par [l'EFF](https://www.eff.org). Nous vous montrons comment procéder dans la première partie de cette documentation : 

Installation de Letsencrypt
---------------------------

Pour installer Letsencrypt, vous devez disposer d'une Debian Jessie ou ultérieure.

Sous Jessie, ajoutez le dépôt Debian de backport dans /etc/apt/sources.list.d comme suit : 
```
echo "deb http://ftp.fr.debian.org/debian jessie-backports main" >/etc/apt/sources.list.d/backports.list
```

puis installez Letsencrypt : 

```
apt update
apt install -t jessie-backports letsencrypt 
```

sur les Debian Stretch ou ultérieur, installez juste Letsencrypt comme suit : 

```
apt update 
apt install letsencrypt
```

une fois Letsencrypt installé, vous devez configurer Apache pour détourner l'alias `/.well-known/acme-challenge` vers un dossier particulier. 
Nous recommandons d'utiliser `/var/www/letsencrypt/.well-known/acme-challenge` pour cela, car c'est le dossier que AlternC-ssl utilisera à l'avenir. 
Pour cela on configure Apache ainsi : 

dans `/etc/apache2/conf-enabled/letsencrypt.conf`, mettez : 

```
Alias /.well-known/acme-challenge /var/www/letsencrypt/.well-known/acme-challenge
```

puis on s'assure que tout est bon : 

```
a2enmod alias
mkdir -p /var/www/letsencrypt/.well-known/acme-challenge
service apache2 reload
```

enfin, on peut demander à Letsencrypt un certificat pour notre panel et serveur : (en remplaçant "alternc.monserveur.com" par son nom DNS réel bien entendu)

```
letsencrypt --certonly --webroot -w /var/www/letsencrypt -d alternc.monserveur.com
```

Letsencrypt vous demandera une adresse email (pour vous prévenir si vous oubliez de renouveler ce certificat), puis vous demandera d'accepter les conditions générales du service.
Enfin, il vous répond normalement que le certificat a été généré dans `/etc/letsencrypt/live/alternc.monserveur.com/fullchain.pem`

En vrai, Letsencrypt a créé 4 fichiers dans ce dossier : ̀`cert.pem` (votre certificat), `privkey.pem` (votre clé privée), `chain.pem` (le certificat chaîné de la CA) et `fullchain.pem` (votre certificat concaténé à celui de la CA)

Apache, postfix, dovecot et proftpd savent bien utiliser le coupe privkey.pem + fullchain.pem. C'est ce couple de fichiers que va utiliser AlternC.

N'oubliez pas de mettre en place un script de renouvellement de ce certificat, soit via certbot (qui le fait tout seul par défaut), soit votre propre script, qui devrait aussi relancer les services si des certificats ont été mis à jour.

Configurer AlternC pour SSL
---------------------------

Lors du lancement de alternc.install, le script regarde si des fichiers `/etc/alternc/fullchain.pem` et `/etc/alternc/privkey.pem` sont présents. S'ils sont présents, il configure les logiciels pour utiliser ces certificats pour leur fonctionnement. 

Aussi, pour utiliser le certificat généré plus haut via Letsencrypt, nous créeons un lien symbolique comme suit : 

```
ln -s /etc/letsencrypt/live/alternc.monserveur.com/fullchain.pem /etc/alternc/fullchain.pem
ln -s /etc/letsencrypt/live/alternc.monserveur.com/privkey.pem /etc/alternc/privkey.pem
```

L'avantage de faire un lien symbolique est qu'en cas de renouvellement, les services reprendront automatiquement le nouveau certificat lors de leur rechargement.

Si vous utilisez un autre système pour vos certificats, changez la cible du lien de manière adéquate.

Une fois ces fichiers présents, relancez `alternc.install`, qui va détecter ces nouveaux fichiers et lancer la configuration du SSL pour les logiciels, qui seront ensuite redémarrés.

Pour vérifier que votre panel a bien HTTPS de configuré, vous pouvez vous rendre sur https://ssllabs.com et entrer votre nom de serveur, une note de A devrait être obtenue automatiquement.



