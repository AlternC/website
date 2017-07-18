
Pre-installation
================

AlternC works well with Linux Debian distribution, either Squeeze, Wheezy (or Jessie as a pre-release).

To install AlternC, you'll need:

* a SSH access to the server.
* administrator rights (`sudo -s` or `su`)
* ensure that `#includedir /etc/sudoers.d` is present in `/etc/sudoers` with the command `visudo`.

Easy Install
============

To install AlternC with just a few questions, [use the easy-install, a shell-script that installs everything quickly](https://github.com/AlternC/easy-install)

You can also install AlternC manually thanks to the documentation below:

Manual Install
==============

ACL
---

AlternC depends on ACLs to manage user rights for the web folders. You should first install the `acl` package with:

```
apt-get install acl
```

Then you need to enable the ACL in the partition that will contain your user data. Change the `/etc/fstab` file by adding the "acl" option to the partition. eg: 

```
/dev/md1    /    ext3    auto,noatime,acl    0    0
```

> WARNING: it's `acl` if your filesystem is `ext4` (or ext3), but it's `attr2` for `xfs`.


Quota
-----

AlternC can also manage disk quotas for each user. As opposed to ACLs, quotas are not mandatory for AlternC to work. If they are not enabled or installed, AlternC will consider that each user has unlimited web space. To enable the quotas, install the quota package: 

```
apt-get install quota
```

Then, change `/etc/fstab` again to enable them:

```
/dev/md1 /               ext3    acl,grpquota,errors=remount-ro 0       1
```

Remounting the partition
------------------------

Then, you need to remount the partition that will contain the user data (in this example, /) with:

```
mount -o remount,acl,grpquota /
```


Installation
============

MySQL
-----

To run AlternC, you need a MySQL server. You can host it on the same machine by using the following command:

```
apt-get install mysql-server
```

> **WARNING** : Enter an administrator password and REMEMBER IT, you'll need it during AlternC install.

You can store this password in a file named `/root/.my.cnf`, with the lines below, that will allow you to connect to your MySQL server by just typing the `mysql` command: 

```
[client]
user=root
password=<your MySQL administrator password>
```


Setup the repositories
----------------------

To install AlternC on a server, you need a text editor to add our repository to your repository list: 

```
deb http://debian.alternc.org/ jessie main
```

Of course, replace "jessie" by the version of Debian you are using (Squeeze, Wheezy, Jessie, Stretch available as of today)

put it in the `/etc/apt/sources.list.d/alternc.list` file.

The Debian packages are digitally signed. Before launching apt-get update, you need to add the pgp key of our repository to your server with the following command:

```
wget https://debian.alternc.org/key.txt -O - | apt-key add -
```

It's a PGP key owned and maintained by the team of developers at AlternC, who have the right to write into our repository at [debian.alternc.org](https://debian.alternc.org).


Installation
------------

Then, update your package list for apt with:

```
apt-get update
```

Then you need to install AlternC (don't forget the  *post install process* later on):

```
apt-get install alternc alternc-ssl alternc-api
```

You can add auxiliary packages of AlternC:

* `alternc-mailman` to manage discussion and mailing lists with [Mailman](http://www.gnu.org/software/mailman/),
* `alternc-roundcube` to use a webmail with [Roundcube](https://roundcube.net/),
* `alternc-awstats` to generate web statistics of your websites with [Awstats](http://www.awstats.org/),

or other AlternC's plugins

DNS - name servers
------------------

The name servers aim is to distribute the information on the domain names installed in your server. You need two different name servers. If you need a secondary name server, AlternC offers you one for free on [alternc.net](https://alternc.net). In that case, you can enter:

* Primary DNS: `ns1.alternc.net`
* Secondary DNS: `ns2.alternc.net`

If you have your own name servers, enter them here. (Usually, the primary is your own server's domain name)

Your Server's Domain Name
-------------------------

Warning: if you have a domain name you want to use to host a website, don't enter it here, because the domain name you enter here will allow you to access your AlternC control panel. 

Use another name pointing to your machine, or a subdomain like `panel.yourdomain.com`.

As an example, if your server has the IP 12.34.56.78 and you have the domain example.com for your personal website, don't enter example.com, but either the IP address, or panel.example.com, or the domain name provided by your hosting provider, such as server234215.bighostingprovider.net

* phpMyAdmin

During the install process, Debian ask you to configure PhpMyAdmin. You can let it do it. It will ask you for the password of the MySQL administrator account to install its database. However, AlternC will change some of those parameters later on.

* Postfix

Choose "Internet Site", then follow the instructions, one of them being the fully qualified domain name of your server (see above)

Install Post Process
--------------------

Once the installation of the package is done, the script `alternc.install` **must be launched**. It will generate configuration files for your server to make AlternC work.

```
alternc.install
```

If you want to install an SSL/TLS certificate for your panel (that will also be used by Dovecot, Proftpd, Postfix etc.) we recommend you to use LetsEncrypt and to configure your certificate [thanks to our online help on SSL/TLS](SSL-en).


Installation End
================

First login
-----------

You can now access your AlternC Panel on the domain name or IP address you entered. You should see a login page whose **default access that you need to change at once** is: 

* login: `admin`
* password: `admin`

Don't forget to change your administrator password by going to the "Manage AlternC account" menu!

