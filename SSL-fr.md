
Installation de SSL/TLS pour AlternC
====================================

Depuis Alternc 3.2.11 (Wheezy), 3.3.11 (Jessie), 3.4.11 (Stretch), un module dédié est proposé [alternc-certbot](https://github.com/alternc/alternc-certbot)

Il prend en charge [Letsencrypt](https://letsencrypt.org) autorité gratuite initiée par [l'EFF](https://www.eff.org) pour l'ensemble des domaines hébergés ainsi que pour le panel.

# Installation

## Wheezy

```shell
apt-get install apt-transport-https
echo "deb [trusted=yes] https://dl.bintray.com/alternc/stable stable main"  >> /etc/apt/sources.list.d/alternc.list
echo 'deb http://download.opensuse.org/repositories/home:/antonbatenev:/letsencrypt/Debian_7.0/ /' > /etc/apt/sources.list.d/certbot.list
apt-get update
apt-get install certbot
apt-get install alternc-certbot
alternc.install
```

## Jessie

```shell
apt-get install apt-transport-https
echo "deb http://ftp.debian.org/debian jessie-backports main" >> /etc/apt/sources.list
echo "deb [trusted=yes] https://dl.bintray.com/alternc/stable stable main"  >> /etc/apt/sources.list.d/alternc.list
apt-get update
apt-get install -t jessie-backports certbot
apt-get install alternc-certbot
alternc.install
```

## Stretch

```shell
apt-get install apt-transport-https
echo "deb [trusted=yes] https://dl.bintray.com/alternc/stable stable main"  >> /etc/apt/sources.list.d/alternc.list
apt-get update
apt-get install certbot alternc-certbot
alternc.install
```

Nous proposons aussi des versions en cours de développement, nous vous invitons à consulter [la documentation du plugin](https://github.com/alternc/alternc-certbot)


Pour les anciennes versions d'Alternc, nous vous invitons à effectuer la mise à jour de votre panel.

## Vérification

Pour vérifier que votre panel a bien HTTPS de configuré, vous pouvez vous rendre sur [SSLLabs](https://ssllabs.com) et entrer votre nom DNS de serveur, une note de *A* devrait être obtenue automatiquement.