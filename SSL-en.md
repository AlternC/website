
Installation of SSL for AlternC
===============================

AlternC currently only manages one SSL certificate for the entire server, that will be used for HTTPS via Apache2 for the Panel, with POP/IMAP in Dovecot, with SMTP in Postfix, and with FTP in ProFtpd.

For this to work, you'll need a valid X.509 certificate, a private key, and if needed (most of the time) a chained certificate for your CA.

We recommend using [Letsencrypt](https://letsencrypt.org), a free certificate authority initiated by [the EFF](https://www.eff.org). We show you how to proceed with Letsencrypt in the first part of this documentation: 

Installation of Letsencrypt
---------------------------

To install Letsencrypt, you need a Jessie or later Debian install.

If you are using Jessie, add the Debian backports repository to your /etc/apt/sources.list.d as follow:
```
echo "deb http://ftp.de.debian.org/debian jessie-backports main" >/etc/apt/sources.list.d/backports.list
```

then install Letsencrypt : 

```
apt update
apt install -t jessie-backports letsencrypt 
```

On Debian Stretch or later, just instal Letsencrypt as follow:

```
apt update 
apt install letsencrypt
```

One Letsencrypt is installed, you need to configure Apache to set the alias `/.well-known/acme-challenge` to point to a specific folder.
We recommend using `/var/www/letsencrypt/.well-known/acme-challenge` since that's what AlternC-ssl is using.
Lets configure Apache. In `/etc/apache2/conf-enabled/letsencrypt.conf`, put the following:

```
Alias /.well-known/acme-challenge /var/www/letsencrypt/.well-known/acme-challenge
```

The we check everything:

```
a2enmod alias
mkdir -p /var/www/letsencrypt/.well-known/acme-challenge
service apache2 reload
```

Then, we can ask Letsencrypt for a certificate for our panel end server: (of course, replace "alternc.myserver.com" by your real DNS name)

```
letsencrypt --certonly --webroot -w /var/www/letsencrypt -d alternc.myserver.com
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



