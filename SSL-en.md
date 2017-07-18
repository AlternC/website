
Installation of SSL/TLS for AlternC
===================================

*Warning, this documentation only applies to version 3.x.11 or later of Alternc. If you are using version 3.x.10 or before, you should use `/etc/alternc/apache.pem`, which is a file with private key, certificate and chained certificated altogether. The rest of the documentation is unchanged.*

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
apt-get update

apt-get install -t jessie-backports letsencrypt 
```

On Debian Stretch or later, just instal Letsencrypt as follow:

```
apt-get update 

apt-get install letsencrypt
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

Letsencrypt will ask you for an email address (to tell you if you forget to renew your certificate), then ask you to accept the terms of services.
Then, it should tell you that your certificate has been generated in `/etc/letsencrypt/live/alternc.myserver.com/fullchain.pem`

In fact, Letsencrypt creates 4 files in this folder : ̀`cert.pem` (your certificate), `privkey.pem` (your private key), `chain.pem` (the chained certificate of the CA) and `fullchain.pem` (your certificate concatenated with the one of the CA)

Apache, postfix, dovecot and proftpd know how to use the privkey.pem + fullchain.pem couple. This couple of files are the ones AlternC will use.

Don't forget to setup a renewal script for this certificate, either via certbot (who does that automatically by default), or your own script, which should also reload the services when a certificate has been updated.

Configure AlternC for SSL
-------------------------

When you launch alternc.install, the script look if the files `/etc/alternc/fullchain.pem` and `/etc/alternc/privkey.pem` are presents. When they are present, it configures the softwares to use that certificate and private key to server their service over TLS

So, to use the certificate you generated above with Letsencrypt, we create the following symbolic links:

```
ln -s /etc/letsencrypt/live/alternc.myserver.com/fullchain.pem /etc/alternc/fullchain.pem

ln -s /etc/letsencrypt/live/alternc.myserver.com/privkey.pem /etc/alternc/privkey.pem
```

The advantage of creating a symlink instead of copying it is that when you'll renew them, the services will automatically use the new certificate when you reload them.

If you use another system for your certificates, you'll need to change the target of the link accordingly.

One those files are present, launch `alternc.install`, which will detect them and launch the configuration of SSL/TLS for the system services, which will then be restarted.

To check that your panel has a proper HTTPS configured, you can go to [SSLLabe](https://ssllabs.com) and enter your server DNS name. A note of *A* should be displayed when the test ends.



