# AlternC's FAQ.

Feel free to contact us on IRC on the channel `#alternc` (on `irc.libera.chat`).

#### In which languages is AlternC available?

AlternC is available in English and French, and partially in Spanish, German and Italian. You can help improving the translatiosn in other languages on [transifex](https://www.transifex.com/octopuce/alternc/).

#### Which Linux Distribution AlternC supports?

AlternC is built for [Debian](https://www.debian.org/). You can currently install it on version 6 (Squeeze) 7 (Wheezy), 8 (Jessie) and 9 (Stretch). The version built for Debian 9 (Stretch) exists but has not been extensively tested as of today.

#### Can we install AlternC on windows?

No.

#### Can we install AlternC on Centos, Fedora, Ubuntu server, Archlinux, etc. (or any other Linux distribution)?

AlternC does not forbid to be installed on other distributions. That said, we don't distribute package for other distributions. This requires to look at source code and Makefile to know where to deploy which configuration, which dependency to install etc.

On any Debian-based distribution (including Debian/KFreeBSD, Devuan or Archlinux), this should work by using AlternC repositories, without any major problem, but this has not been tested.

#### I put wrong answers during the install, what can I do?

The easiest method is to run the command `dpkg-reconfigure -plow alternc` then `alternc.install`. 

You can also change the configuration file `/etc/alternc/alternc.conf` then (re)launch `alternc.install`.

#### How can I upgrade to latest version?

As for the installation, you need to ensure you have AlternC Debian package repository setup in `/etc/apt/sources.list.d/alternc.list`:

```
deb http://debian.alternc.org/ jessie main
```

and add our repository key if you didn't already do it: 

```
wget https://debian.alternc.org/key.txt -O - | apt-key add -
```

Then you can update your package list (`apt-get update`) and launch the upgrade with `apt-get upgrade`.

#### I can't create a domain/FTP account/mailing list/etc…

It's probably a quota issue, where you have no quota or expired your quota for this service. Quotas are managed by the administrator account, in the "Manage members" menu. We also advise you to change the default quotas if needed in the "administrator panel".

#### I just installed a plugin but no menu appears in my account!

It's probably a quota issue for this service, which is certainly at 0 (see previous answer).

#### What is the correspondance between AlternC's account IDs and UNIX users?

Each AlternC account has a Unix user and group. These group and user are currently not defined in `/etc/group` or `/etc/passwd`, therefore, the files appears as having numerical IDs (such as 2001:2002).

#### I lost my administrator password!

You can reset your administrator password (the admin account) via the command line `/usr/lib/alternc/alternc-passwd`.



